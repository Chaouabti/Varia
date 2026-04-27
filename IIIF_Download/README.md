# IIIF Manuscript Downloader

A Jupyter notebook for downloading manuscript images from [IIIF](https://iiif.io/) manifests. Supports both IIIF v2 and v3.

## Requirements

```bash
pip install requests Pillow pathlib pandas
```

## Input

The notebook takes a CSV file as input with at least two columns, separated by `;`:

| MS_name | Manifest_URL |
|---|---|
| Add_11283 | https://bl.digirati.io/iiif/ark:/81055/vdc_100055965341.0x000001 |
| MSS_Reg.lat.258 | https://digi.vatlib.it/iiif/MSS_Reg.lat.258/manifest.json |

- `MS_name` : the name used for the output folder and files
- `Manifest_URL` : the URL of the IIIF manifest

## Usage

Set the following variables in the notebook and run `download_data()`:

```python
manifests_csv = 'path/to/your/manifests_list.csv'
data_folder = 'path/to/your/output/folder'
requests_pause = 5  # seconds to wait after a 429 error
download_data(manifests_csv, data_folder, requests_pause)
```

## Output

Each manuscript generates a dedicated folder containing:

- the IIIF manifest saved locally (to avoid repeated requests)
- all downloaded images
- a CSV file documenting the download for each image

```
data_folder/
├── MS_name_1/
│   ├── MS_name_1.json
│   ├── MS_name_1_image_data.csv
│   ├── image_1.jpg
│   ├── image_2.jpg
│   └── ...
├── MS_name_2/
│   ├── MS_name_2.json
│   ├── MS_name_2_image_data.csv
│   ├── image_1.jpg
│   └── ...
```

The `_image_data.csv` file contains the following columns:

| Column | Description |
|---|---|
| canvasNum | canvas index in the manifest |
| imageLabel | image label as declared in the manifest |
| urlImage | image URL |
| imageFormat | image format (jpg, png…) |
| imageWidthAsDeclared | width declared in the manifest |
| imageHeightAsDeclared | height declared in the manifest |
| imageFileName | downloaded file name |
| folderPath | path to the folder |
| httpCode | HTTP status code (200, 429, Exception…) |
| imageWidthAsDownloaded | actual width after download |
| imageHeightAsDownloaded | actual height after download |

Images that failed to download (httpCode ≠ 200) are logged in this CSV and can be retried by re-running the notebook — already downloaded images are skipped automatically.

## Author

[@Chaouabti](https://github.com/Chaouabti)

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
