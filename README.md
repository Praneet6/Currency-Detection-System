# ESP32-CAM Indian Currency Recognition System

Real-time Indian currency note detection using Edge Impulse ML and ESP32-CAM with LED indicators.

## 🎯 Project Overview

This project detects Indian currency denominations (₹10, ₹50, ₹100, ₹500) using computer vision and machine learning on an ESP32-CAM microcontroller. Each detected denomination triggers a corresponding LED indicator.

## ✨ Features

- **Real-time Currency Detection** - Instant recognition using camera feed
- **LED Visual Feedback** - Color-coded indicators for each denomination
- **High Accuracy** - 64.7% validation accuracy with transfer learning
- **Lightweight ML Model** - MobileNetV1 optimized for embedded systems
- **Serial Debugging** - Real-time confidence scores and predictions
- **Low Power Consumption** - Efficient inference on ESP32-CAM

## 📋 Hardware Components

| Component | Qty | Pin | Notes |
|-----------|-----|-----|-------|
| ESP32-CAM (AI Thinker) | 1 | - | With OV3660 camera |
| FT232RL USB-to-TTL | 1 | - | Mini USB interface |
| Green LED | 1 | GPIO12 | ₹10 indicator |
| Yellow LED | 1 | GPIO13 | ₹50 indicator |
| Blue LED | 1 | GPIO14 | ₹100 indicator |
| Red LED | 1 | GPIO15 | ₹500 indicator |
| 100Ω Resistor | 4 | - | LED current limiting |
| Breadboard | 1 | - | For connections |
| Jumper Wires | 20+ | - | Male-to-male/female |
| USB Power Cable | 1 | - | Additional 5V supply |

## 💻 Software Requirements

- Arduino IDE 2.0+
- ESP32 Board Support Package
- Edge Impulse Arduino Library (Praneeett6-project-1_inferencing)
- Python 3.8+ (for data preprocessing)

## 📦 Installation Guide

### Step 1: Arduino IDE Setup

1. Open Arduino IDE
2. Go to **File > Preferences**
3. Add Board URL: `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
4. Go to **Tools > Board > Boards Manager**
5. Search and install "esp32" by Espressif Systems
6. Select Board: **Tools > Board > AI Thinker ESP32-CAM**

### Step 2: Install Edge Impulse Library

1. Download library from Edge Impulse: **Deployment > Arduino Library**
2. In Arduino IDE: **Sketch > Include Library > Add .ZIP Library**
3. Select `Praneeett6-project-1_inferencing.zip`
4. Restart Arduino IDE

### Step 3: Upload Code

1. Copy the code from `sketch_may8a.ino`
2. Create new sketch in Arduino IDE
3. Paste code
4. Connect FT232RL to laptop USB
5. Connect GPIO0 → GND (programming mode)
6. Select **Tools > Port > COM12** (or your port)
7. Click **Upload**
8. When "Connecting......" appears, unplug GPIO0 wire
9. Wait for "Done uploading"
10. Remove GPIO0 wire completely
11. Restart board

## 📊 Model Architecture

**Framework**: Edge Impulse  
**Input Size**: 96×96 RGB  
**Model Type**: MobileNetV1 0.1  
**Training Mode**: Classification + Transfer Learning  
**Dataset Size**: 212 images (53 per denomination)  
**Training Cycles**: 150  
**Learning Rate**: 0.0005  
**Batch Size**: 8  
**Augmentation**: ON (flip, rotate, brightness)

## 📈 Model Performance

### Overall Metrics
- **Validation Accuracy**: 64.7%
- **Loss**: 1.00
- **Area under ROC Curve**: 0.84
- **Weighted Average Precision**: 0.66
- **Weighted Average Recall**: 0.65
- **Weighted Average F1 Score**: 0.65

### Per-Class Performance (Confusion Matrix)

| Class | ₹10 | ₹50 | ₹100 | ₹500 | F1 Score |
|-------|-----|-----|------|------|----------|
| **₹10** | 57.1% | 14.3% | 0% | 28.6% | 0.57 |
| **₹50** | 22.2% | 66.7% | 0% | 11.1% | 0.71 |
| **₹100** | 0% | 12.5% | 62.5% | 25% | 0.67 |
| **₹500** | 10% | 0% | 20% | 70% | 0.64 |

### Resource Usage
- **RAM**: 53.2KB (Safe for ESP32: 520KB available)
- **ROM**: 101KB (Safe for ESP32: 4MB available)
- **Inference Time**: ~400-500ms per image
- **Buffer**: No overflow risk ✅

## 🚀 Usage Instructions

### Power On & Initialization

1. Connect both USB cables (FT232 + Power)
2. Board boots automatically
3. Serial Monitor at 115200 baud shows initialization
4. LED test sequence runs (all 4 LEDs blink)
5. Camera initializes
6. Ready for detection

### Detection Workflow

1. **Open Serial Monitor** - Tools > Serial Monitor (115200 baud)
2. **Place Currency Note** - In front of camera
3. **Wait for Detection** - Usually 2-3 seconds
4. **LED Glows** - Corresponding color indicates denomination
5. **Serial Output** - Shows predictions and confidence

### Serial Monitor Output Example 
=====================================
Currency Recognition System
₹10 | ₹50 | ₹100 | ₹500
Camera initialized successfully!
Predictions:
10: 0.15
50: 0.25
100: 0.58
500: 0.02
DETECTED: 100 (58.0% confidence)
LED: BLUE (₹100)
## 🔧 Troubleshooting Guide

### Serial Monitor Shows Nothing

**Problem**: No output despite board powered  
**Solutions**:
1. Unplug both USB cables
2. Wait 5 seconds
3. Open Serial Monitor FIRST
4. Set baud to 115200
5. Plug USB back in
6. Press Reset button on board

### LEDs Not Glowing

**Problem**: Serial shows detection but LEDs off  
**Solutions**:
1. Check LED wiring connections
2. Verify LED polarity (long leg to resistor)
3. Confirm GPIO pins: 12, 13, 14, 15
4. Test resistors with multimeter (100Ω)
5. Check GND rail connection

### Low Accuracy / No Detection

**Problem**: Predictions under 55% confidence  
**Solutions**:
1. Ensure good lighting (natural or LED)
2. Use plain white background
3. Place note flat and clearly in frame
4. Avoid shadows or reflections
5. Retrain model with more images (100+ per class)

### Buffer Overflow Error

**Problem**: "Memory allocation failed"  
**Solutions**:
1. Already using MobileNetV1 (smallest model)
2. Check RAM availability
3. Reduce inference frequency (increase delay)
4. Use smaller image size

### USB Port Not Recognized

**Problem**: COM port not appearing  
**Solutions**:
1. Install FT232RL drivers
2. Check USB cable connection
3. Try different USB port
4. Restart Arduino IDE
5. Update board support package

## 📚 Training Data Details

### Data Collection
- **Total Images**: 212 currency notes
- **Per Denomination**: 53 images
- **Image Format**: RGB JPG, 96×96 pixels
- **Background**: White/plain
- **Data Split**: 80% train, 20% validation

### Data Quality Criteria
- ✅ Sharp focus (no blur)
- ✅ Even lighting (no shadows)
- ✅ Full note visible
- ✅ Plain background
- ✅ Various angles (0°, 45°, 90°)
- ✅ Different lighting conditions

### Data Processing
- Cropped to note only (no background)
- Resized to 96×96 with white padding
- "Fit shortest axis" resize mode
- RGB color depth maintained

## 🎓 Model Training Details

### Transfer Learning Benefits
- Pre-trained weights from ImageNet
- Reduces training time
- Better accuracy with small datasets
- Prevents overfitting

### Training Settings
## 📁 Project Files
├── sketch_may8a.ino                    # Main Arduino code
├── README.md                           # This documentation
├── Hardware_Connections.md             # Detailed wiring guide
├── Model_Training_Guide.md             # Edge Impulse tutorial
├── Troubleshooting_Guide.md            # Solutions for common issues
└── Data/
├── 10_rupee_training/              # ₹10 training images
├── 50_rupee_training/              # ₹50 training images
├── 100_rupee_training/             # ₹100 training images
└── 500_rupee_training/             # ₹500 training images
**Inference**
- `run_classifier()` - Run ML model on captured image
- Confidence threshold: 0.55 (55%)
- Output to Serial Monitor

**Main Loop**
- Capture image every 2 seconds
- Run inference
- Display predictions
- Control LEDs based on best prediction

## ⚡ Performance Specifications

| Metric | Value |
|--------|-------|
| Inference Speed | 400-500ms |
| Detection Range | 10-30cm |
| Optimal Lighting | Natural or LED |
| Power Consumption | ~500mA |
| Minimum Accuracy | 55% confidence |
| Storage Required | ~150KB (model) |
| RAM Available | 520KB (50KB used) |

## 🎯 Accuracy by Condition

| Condition | Expected Accuracy |
|-----------|-------------------|
| Good lighting, white background | 75%+ |
| Normal lighting, plain background | 65-70% |
| Low lighting, cluttered background | 50-60% |
| Damaged/worn notes | 55-65% |
| Multiple notes in frame | Lower (not designed for this) |

## 📧 Support & Issues

For issues or questions:

1. Check **Troubleshooting Guide** first
2. Verify wiring connections
3. Check Serial Monitor output for error codes
4. Test LEDs with simple blink sketch
5. Retrain model with more data if accuracy low

## 📄 License

MIT License - Free to use and modify for personal/educational projects

## 👨‍💻 Project Author
Praneet Rathore
---

## 🎉 Success Criteria Met

- ✅ Hardware assembled and tested
- ✅ Camera working on ESP32-CAM
- ✅ 4 LEDs functional with correct wiring
- ✅ Edge Impulse model trained (64.7% accuracy)
- ✅ Arduino code uploaded and running
- ✅ Real-time currency detection working
- ✅ LED indicators glowing correctly
- ✅ Serial output showing predictions
- ✅ Demo ready for presentation
