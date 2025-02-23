# SEEG-Planning

# SEEG Trajectory Planning Web Application

## Overview

This project provided a **web-based platform** for **automated stereoelectroencephalography (SEEG) trajectory planning**. It accepted **DICOM files**, processed them into **NIFTI format**, applied an **AI-based trajectory planning algorithm**, and displayed the planned trajectories overlaid on MRI scans in **2D and 3D**. Users uploaded **DICOM images**, visualized the planned trajectories, and downloaded the processed **NIFTI files**.

## Features

- **DICOM to NIFTI Conversion**: Converted uploaded DICOM files into NIFTI format.
- **AI-Based SEEG Planning**: Applied the **Multiple Trajectory Planning Algorithm (MTP)** with AI-based risk assessment.
- **Trajectory Overlay on MRI**: Displayed **optimized electrode paths** while avoiding blood vessels and critical structures.
- **3D Visualization**: Rendered MRI scans and planned SEEG trajectories in **3D** using **vtk.js**.
- **User-Friendly Web Interface**: Allowed users to upload DICOM files, view results, and download processed NIFTI outputs.
- **Free Hosting**: Hosted using **GitHub Pages (Frontend)** and **Railway.app (Backend)**.

## Tech Stack 🚀

### **Backend (Flask API) 🖥️🐍**

- **Python 3**
- **Flask** (REST API)
- **pydicom** (DICOM processing)
- **nibabel** (NIFTI conversion)
- **NumPy** (Array processing)
- **Matplotlib** (Trajectory visualization)
- **OpenCV** (Image manipulation)
- **Railway.app** (Free backend hosting)

### **Frontend (React.js Web App) 🌐⚛️**

- **React.js** (Frontend framework)
- **axios** (API communication)
- **vtk.js** (3D visualization)
- **TailwindCSS** (Styling)
- **GitHub Pages** (Free frontend hosting)

## Installation and Setup 🔧

### **1. Clone the Repository**

```sh
git clone https://github.com/Abhiram-Sharma/SEEG-Planning.git
cd SEEG-Planning
```

### **2. Backend Setup (Flask API)**

#### **Install Dependencies**

```sh
cd backend
pip install -r requirements.txt
```

#### **Run Flask API Locally**

```sh
python app.py
```

- API ran at **[http://127.0.0.1:5000](http://127.0.0.1:5000)**.
- Uploaded DICOM files were processed into **NIFTI format**.
- The AI-based planning algorithm computed **safe SEEG trajectories**.
- Trajectory visualizations were generated and stored as PNG images.

#### **Deploy Flask API on Railway.app**

1. Created an account on [Railway.app](https://railway.app/).
2. Created a new project and linked the **backend folder**.
3. Set `app.py` as the main entry point.
4. Obtained a **free public API URL**.

### **3. Frontend Setup (React.js Web App)**

#### **Install Dependencies**

```sh
cd ../frontend
npm install
```

#### **Run React Frontend Locally**

```sh
npm start
```

- Opened **[http://localhost:3000/](http://localhost:3000/)** in the browser.
- Allowed users to **upload DICOM files**, **view planned trajectories**, and **download processed NIFTI files**.

#### **Deploy Frontend on GitHub Pages**

```sh
npm run build
npx gh-pages -d build
```

- Hosted at `https://Abhiram-Sharma.github.io/SEEG-Planning`.

## API Endpoints

### **Upload DICOM Files**

```http
POST /upload
```

**Request:**

- Content-Type: `multipart/form-data`
- Body: `dicom_files[]` (Multiple DICOM files)

**Response:**

- Processed **NIFTI file (output.nii.gz)**.

### **Download Processed Files**

```http
GET /download/<filename>
```

- Provided **NIFTI files + PNG overlays** for download.

## AI-Based Trajectory Planning Algorithm 🤖🧠

- Used **Dynamic Programming + Depth-First Search (DFS)** for optimized SEEG trajectory planning.
- Avoided **blood vessels and critical brain structures**.
- Ensured **maximum gray matter sampling**.

### **Trajectory Computation Code** (Python)

```python
import numpy as np

def compute_safe_trajectory(entry_points, targets, vessel_map):
    trajectories = []
    for entry, target in zip(entry_points, targets):
        path = np.linspace(entry, target, num=100)
        risk = np.sum(vessel_map[path.astype(int)])
        if risk < 5:
            trajectories.append((entry, target))
    return trajectories
```

## Visualization (2D & 3D)

- Used **Matplotlib + OpenCV** for 2D trajectory overlays.
- Used **vtk.js** for 3D visualization.

### **Example 2D Overlay Code**

```python
import nibabel as nib
import matplotlib.pyplot as plt

def visualize_trajectory(nifti_path, trajectory, output_image):
    nifti_img = nib.load(nifti_path)
    img_data = nifti_img.get_fdata()
    plt.imshow(img_data[:, :, img_data.shape[2]//2], cmap='gray')
    for entry, target in trajectory:
        plt.plot([entry[0], target[0]], [entry[1], target[1]], 'r-', linewidth=2)
    plt.savefig(output_image)
    plt.show()
```

### **Example 3D Viewer Code (React + vtk.js)**

```javascript
import React, { useEffect } from "react";
import vtkGenericRenderWindow from "@kitware/vtk.js/Rendering/Misc/GenericRenderWindow";

const Viewer = () => {
  useEffect(() => {
    const renderWindow = vtkGenericRenderWindow.newInstance();
    renderWindow.setContainer(document.getElementById("viewer"));
    renderWindow.getRenderer().setBackground(0, 0, 0);
  }, []);
  return <div id="viewer" style={{ width: "100%", height: "500px" }}></div>;
};
export default Viewer;
```

## Future Enhancements

- **Improve AI model** by incorporating **deep learning-based segmentation**.
- **Add user accounts** with patient data history.
- **Integrate automatic cloud-based storage** for DICOM uploads.

## Contributors

- **Abhiram** **Sharma** - *Backend Developer*
- **Saksham Anand** - *Frontend Developer*
- **Krishna Priyadarshan Behara** - *Cloud Developer*

## License

This project was released under the **MIT License**.

## Contact

For support or contributions, open an issue on [GitHub](https://github.com/YOUR_GITHUB_USERNAME/SEEG-Planning/issues).

