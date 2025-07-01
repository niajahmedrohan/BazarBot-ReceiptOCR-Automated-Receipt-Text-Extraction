# Receipt OCR - Automated Receipt Text Extraction

A Python-based computer vision project that automatically detects, processes, and extracts text from receipt images using OpenCV and Tesseract OCR. The system can identify receipt boundaries, correct perspective distortion, and extract itemized food purchases with their corresponding costs.

## Features

- **Automatic Receipt Detection**: Identifies receipt boundaries within images using contour detection
- **Perspective Correction**: Automatically corrects skewed or angled receipt images
- **Image Preprocessing**: Applies morphological operations and filtering to optimize OCR performance
- **Text Extraction**: Uses Tesseract OCR to extract text from processed images
- **Data Parsing**: Intelligently separates food items from prices using regex patterns
- **Structured Output**: Presents results in a clean, tabulated format

## Requirements

### Dependencies

```bash
pip install opencv-python
pip install pytesseract
pip install numpy
pip install matplotlib
pip install scikit-image
pip install Pillow
pip install prettytable
```

### System Requirements

- **Tesseract OCR**: Must be installed on your system
  - Windows: Download from [GitHub releases](https://github.com/tesseract-ocr/tesseract/releases)
  - macOS: `brew install tesseract`
  - Linux: `sudo apt-get install tesseract-ocr`

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/receipt-ocr.git
cd receipt-ocr
```

2. Install required Python packages:
```bash
pip install -r requirements.txt
```

3. Ensure Tesseract is properly installed and accessible from your PATH.

## Usage

### Basic Usage

```python
# Set your image file path
file_name = 'path/to/your/receipt.png'

# Run the complete pipeline
# The script will automatically:
# 1. Detect receipt contours
# 2. Apply perspective correction
# 3. Preprocess for OCR
# 4. Extract and parse text
# 5. Display results in a table
```

### Key Functions

#### Image Processing
- `opencv_resize(image, ratio)`: Resize images for optimal processing
- `approximate_contour(contour)`: Simplify contours to basic polygon shapes
- `get_receipt_contour(contours)`: Find rectangular receipt boundaries
- `wrap_perspective(img, rect)`: Correct perspective distortion

#### OCR and Text Processing
- `bw_scanner(image)`: Convert to high-contrast binary image
- `find_amounts(text)`: Extract monetary values using regex
- Text cleaning and parsing pipeline for food items and costs

## How It Works

### 1. Receipt Detection
- Converts image to grayscale
- Applies Gaussian blur to reduce noise
- Uses Canny edge detection to find edges
- Identifies the largest rectangular contour as the receipt

### 2. Perspective Correction
- Finds four corner points of the receipt
- Calculates transformation matrix
- Warps the image to create a "scanned" appearance

### 3. OCR Preprocessing
- Applies threshold operations for high contrast
- Uses morphological operations (dilation, erosion)
- Optimizes image for Tesseract OCR performance

### 4. Text Extraction and Parsing
- Extracts all text using Tesseract OCR
- Filters lines containing prices (decimal numbers)
- Removes common non-food items (tax, total, etc.)
- Separates food items from their costs
- Presents results in a formatted table

## Configuration

### OCR Settings
The script uses custom Tesseract configuration:
```python
custom_config = r'--oem 3 --psm 6'
```

### Filtering Lists
Customize the exclusion and removal lists for your specific needs:
```python
exclusion_list = ["bank", "total", "promo", "vat", "change", "recyclable"]
remove_list = ["vit", "etc"]
```

## Input Requirements

### Image Quality
- **High Resolution**: Works best with high-resolution images
- **Good Lighting**: Ensure receipt is well-lit and readable
- **Clear Background**: Images with contrasting backgrounds work better
- **Minimal Skew**: While perspective correction is applied, less skewed images yield better results

### Supported Formats
- PNG, JPG, JPEG
- Works with both white and gradient backgrounds
- Handles various receipt sizes and orientations

## Output

The program generates:
1. **Processed Images**: Visual confirmation of each processing step
2. **Extracted Text**: Raw OCR output
3. **Structured Table**: Clean presentation of food items and costs

Example output:
```
+------------------+-------+
|    Food Item     | Cost  |
+------------------+-------+
| APPLE JUICE      | 2.50  |
| BREAD LOAF       | 3.25  |
| MILK CARTON      | 4.99  |
+------------------+-------+
```

## Limitations

- Performance depends heavily on image quality and resolution
- May struggle with very low contrast or damaged receipts
- OCR accuracy can vary with different fonts and print qualities
- Currently optimized for English text receipts

## Performance Tips

1. **Image Quality**: Use high-resolution, well-lit images
2. **Preprocessing**: The morphological operations significantly improve OCR accuracy
3. **Background**: Ensure good contrast between receipt and background
4. **Orientation**: While rotation is handled, upright images work best

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

## Acknowledgments

- OpenCV community for computer vision tools
- Tesseract OCR project for text recognition capabilities
- scikit-image for advanced image processing functions

## Troubleshooting

### Common Issues

**Tesseract not found error:**
- Ensure Tesseract is installed and added to your system PATH
- On Windows, you may need to specify the path explicitly:
```python
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'
```

**Poor OCR results:**
- Try increasing image resolution
- Ensure good lighting and contrast
- Check that the receipt is properly detected and cropped

**No contours detected:**
- Verify the receipt has sufficient contrast with the background
- Try adjusting the Canny edge detection parameters
- Ensure the receipt occupies a significant portion of the image
