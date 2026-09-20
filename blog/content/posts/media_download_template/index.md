---
title: <Template> Script Template For Asyncronous Media File Download from URL
date: 2025-11-02T18:33:20Z
lastmod: 2025-11-09T18:33:20Z
author: Jun Yeop(Johnny) Na
avatar: /images/favicon.svg
# authorlink: https://author.site
cover: cover.png
category: General Dev
categories:
  - AI
  - Fashion
tags:
  - Computer Vision
  - AI Model
  - Fine Tuning
# nolastmod: true
draft: false
---

Usually Media data(Audio, Video, Image) are stored as URLs in the dataset, because saving the whole media data in the dataset will be very heavy.

So every time I work with Media Dataset I have to first download them to my local directory for fast use. I use the following code as template, and change specifics depending on the project's needs

# Single Process

- Simple and easy-to read

```python
import json
import os
import requests
from pathlib import Path
from urllib.parse import urlparse
from tqdm import tqdm
import time


def download_image(url, save_path, max_retries=3):
    """Download an image from URL with retry logic."""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=30)
            response.raise_for_status()

            with open(save_path, 'wb') as f:
                f.write(response.content)
            return True
        except Exception as e:
            if attempt < max_retries - 1:
                time.sleep(1)  # Wait before retry
                continue
            print(f"Failed to download {url}: {e}")
            return False
    return False


def get_image_extension(url, image_id):
    """Get image extension from URL or default to jpg."""
    parsed = urlparse(url)
    path = parsed.path.lower()

    # Check common image extensions in URL
    for ext in ['.jpg', '.jpeg', '.png', '.gif', '.webp']:
        if ext in path:
            return ext

    # Default to jpg if no extension found
    return '.jpg'


def download_images_from_json(json_path, output_dir, json_name="dataset"):
    """Download images from a JSON file."""
    print(f"\nLoading {json_name}...")

    # Read JSON file
    with open(json_path, 'r', encoding='utf-8') as f:
        data = json.load(f)

    # Extract images array
    if isinstance(data, dict) and 'images' in data:
        images = data['images']
    elif isinstance(data, list):
        images = data
    else:
        print(f"Unexpected JSON structure in {json_path}")
        return

    # Create output directory
    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)

    print(f"Found {len(images)} images to download")
    print(f"Saving to: {output_path.absolute()}")

    # Download images with progress bar
    successful = 0
    failed = 0

    for img_data in tqdm(images, desc=f"Downloading {json_name}"):
        url = img_data.get('url')
        image_id = img_data.get('imageId', 'unknown')

        if not url:
            failed += 1
            continue

        # Determine file extension and create filename
        ext = get_image_extension(url, image_id)
        filename = f"{image_id}{ext}"
        save_path = output_path / filename

        # Skip if already downloaded
        if save_path.exists():
            successful += 1
            continue

        # Download the image
        if download_image(url, save_path):
            successful += 1
        else:
            failed += 1

    print(f"\n{json_name} Summary:")
    print(f"  Successful: {successful}")
    print(f"  Failed: {failed}")
    print(f"  Total: {len(images)}")


def main():
    """Main function to download images from train.json and validation.json."""
    script_dir = Path(__file__).parent

    # Paths
    train_json = script_dir / "train.json"
    validation_json = script_dir / "validation.json"
    train_dir = script_dir / "train"
    validation_dir = script_dir / "validation"

    # Download train images
    if train_json.exists():
        download_images_from_json(train_json, train_dir, "train.json")
    else:
        print(f"Warning: {train_json} not found")

    # Download validation images
    if validation_json.exists():
        download_images_from_json(validation_json, validation_dir, "validation.json")
    else:
        print(f"Warning: {validation_json} not found")

    print("\nDownload complete!")


if __name__ == "__main__":
    main()

```

Simple and easy-to-read. When I run the code it takes **600hours to download 1Million Images**

# Multi Thread Script

Since Image Download is a IO-bound task, we can use multi-threading to speed up the download

```python
import json
import os
import requests
from pathlib import Path
from tqdm import tqdm
import time
from concurrent.futures import ThreadPoolExecutor, as_completed
from functools import partial


def download_image(url, save_path, max_retries=1):
    """Download an image from URL with retry logic."""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=30)
            response.raise_for_status()

            with open(save_path, 'wb') as f:
                f.write(response.content)
            return True
        except Exception as e:
            if attempt < max_retries - 1:
                time.sleep(1)  # Wait before retry
                continue
            return False
    return False


def download_single_image(img_data, output_dir):
    """Download a single image - worker function for threading."""
    url = img_data.get('url')
    image_id = img_data.get('imageId', 'unknown')

    if not url:
        return {'success': False, 'image_id': image_id}

    # Create filename with .png extension
    filename = f"{image_id}.png"
    save_path = Path(output_dir) / filename

    # Skip if already downloaded
    if save_path.exists():
        return {'success': True, 'image_id': image_id, 'skipped': True}

    # Download the image
    if download_image(url, save_path):
        return {'success': True, 'image_id': image_id, 'skipped': False}
    else:
        return {'success': False, 'image_id': image_id}


def download_images_from_json(json_path, output_dir, json_name="dataset"):
    """Download images from a JSON file."""
    print(f"\nLoading {json_name}...")

    # Read JSON file
    with open(json_path, 'r', encoding='utf-8') as f:
        data = json.load(f)

    # Extract images array
    if isinstance(data, dict) and 'images' in data:
        images = data['images']
    elif isinstance(data, list):
        images = data
    else:
        print(f"Unexpected JSON structure in {json_path}")
        return

    # Create output directory
    output_path = Path(output_dir)
    output_path.mkdir(parents=True, exist_ok=True)

    print(f"Found {len(images)} images to download")
    print(f"Saving to: {output_path.absolute()}")

    # Download images with threading
    successful = 0
    failed = 0
    skipped = 0

    # Create worker function with output_dir bound
    worker = partial(download_single_image, output_dir=str(output_path))

    # Use threading to download images in parallel
    # For I/O-bound tasks, we can use more threads than CPU cores
    num_workers = min(32, (os.cpu_count() or 4) * 4)  # Up to 32 threads for I/O-bound tasks

    with ThreadPoolExecutor(max_workers=num_workers) as executor:
        # Submit all download tasks
        future_to_img = {executor.submit(worker, img_data): img_data for img_data in images}

        # Process completed downloads with progress bar
        with tqdm(total=len(images), desc=f"Downloading {json_name}") as pbar:
            for future in as_completed(future_to_img):
                result = future.result()
                if result['success']:
                    successful += 1
                    if result.get('skipped', False):
                        skipped += 1
                else:
                    failed += 1
                pbar.update(1)

    print(f"\n{json_name} Summary:")
    print(f"  Successful: {successful} (skipped: {skipped}, downloaded: {successful - skipped})")
    print(f"  Failed: {failed}")
    print(f"  Total: {len(images)}")


def main():
    """Main function to download images from train.json and validation.json."""
    script_dir = Path(__file__).parent

    # Paths
    train_json = script_dir / "train.json"
    validation_json = script_dir / "validation.json"
    train_dir = script_dir / "train"
    validation_dir = script_dir / "validation"

    # Download train images
    if train_json.exists():
        download_images_from_json(train_json, train_dir, "train.json")
    else:
        print(f"Warning: {train_json} not found")

    # Download validation images
    if validation_json.exists():
        download_images_from_json(validation_json, validation_dir, "validation.json")
    else:
        print(f"Warning: {validation_json} not found")

    print("\nDownload complete!")


if __name__ == "__main__":
    main()
```

- This reduces my download time from 600hours -> 15hours.
