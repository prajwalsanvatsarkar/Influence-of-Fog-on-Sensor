# Sensor Data Analysis

This repository contains code for analyzing LiDAR and radar sensor data captured under clear and foggy conditions. The sensors used in this analysis are Blickfeld, radar, and Velodyne.

## Table of Contents
1. [Folder Structure](#folder-structure)
2. [Code Overview](#code-overview)
    - [Setting Up Folder Paths](#setting-up-folder-paths)
    - [Reading Data](#reading-data)
    - [Extracting and Printing Basic Data Information](#extracting-and-printing-basic-data-information)
    - [Data Range Analysis](#data-range-analysis)
    - [Distance Calculation](#distance-calculation)
    - [Data Shape Verification](#data-shape-verification)
    - [Plotting Histograms](#plotting-histograms)
3. [Summary](#summary)
4. [Prerequisites](#prerequisites)
5. [Usage](#usage)
6. [License](#license)

## Folder Structure

The data is organized into the following folder structure:

E:/Sem 1 Notes/Lidar And Radar Systems/Task 1/CBuilding/CBuilding/csv/
├── c_building_pedestrian_clear_anon/
│ ├── blickfeld/
│ ├── radar/
│ ├── velodyne/
├── c_building_pedestrian_fog_anon/
├── blickfeld/
├── radar/
├── velodyne/



## Code Overview

### Setting Up Folder Paths
The code begins by defining the sensors and the paths to the data folders for clear and foggy conditions.

```python
sensors = ["blickfeld", "radar", "velodyne"]

folder_paths_all = {
    "clear": "E:/Sem 1 Notes/Lidar And Radar Systems/Task 1/CBuilding/CBuilding/csv/c_building_pedestrian_clear_anon/",
    "fog": "E:/Sem 1 Notes/Lidar And Radar Systems/Task 1/CBuilding/CBuilding/csv/c_building_pedestrian_fog_anon/"
}
```

### Reading Data
The code iterates over each sensor and reads the data from the specified directories into a NumPy array. The data is then concatenated to form a single array for each sensor under each condition.
```python
data = {}

for condition, folder_path in folder_paths_all.items():
    data[condition] = {}

    for sensor in sensors:
        sensor_path = Path(folder_path) / sensor
        files = [file for file in sensor_path.iterdir() if file.is_file() and file.suffix == '.csv']

        print(files)

        sensor_data = []

        for file in files:
            print(file)
            arr = pd.read_csv(file, delimiter=" ").values
            print(arr.shape)

            print(f"Reading {sensor} data for {file}")
            sensor_data.append(arr)

        data[condition][sensor] = np.concatenate(sensor_data, axis=0)

```

### Extracting and Printing Basic Data Information
The code extracts the data arrays for each sensor and condition, then prints their shapes to verify the data has been read correctly.
```python
blickfeld_data_clear = data["clear"]["blickfeld"]
radar_data_clear = data["clear"]["radar"]
velodyne_data_clear = data["clear"]["velodyne"]

blickfeld_data_fog = data["fog"]["blickfeld"]
radar_data_fog = data["fog"]["radar"]
velodyne_data_fog = data["fog"]["velodyne"]

print(blickfeld_data_clear.shape)
print(radar_data_clear.shape)
print(velodyne_data_clear.shape)
print(blickfeld_data_fog.shape)
print(radar_data_fog.shape)
print(velodyne_data_fog.shape)
```

### Data Range Analysis
The first three columns (x, y, z coordinates) from each sensor's data are extracted. The maximum and minimum values for these coordinates are printed, providing a range analysis
```python
blickfeld_data_clear = blickfeld_data_clear[:, 0:3]
print("blickfeld_data_clear: max = ", blickfeld_data_clear.max(axis=0), " ,min = ", blickfeld_data_clear.min(axis=0))
radar_data_clear = radar_data_clear[:, 0:3]
print("radar_data_clear: max = ", radar_data_clear.max(axis=0), " ,min = ", radar_data_clear.min(axis=0))
velodyne_data_clear = velodyne_data_clear[:, 0:3]
print("velodyne_data_clear: max = ", velodyne_data_clear.max(axis=0), " ,min = ", velodyne_data_clear.min(axis=0))
blickfeld_data_fog = blickfeld_data_fog[:, 0:3]
print("blickfeld_data_fog: max = ", blickfeld_data_fog.max(axis=0), " ,min = ", blickfeld_data_fog.min(axis=0))
radar_data_fog = radar_data_fog[:, 0:3]
print("radar_data_fog: max = ", radar_data_fog.max(axis=0), " ,min = ", radar_data_fog.min(axis=0))
velodyne_data_fog = velodyne_data_fog[:, 0:3]
print("velodyne_data_fog: max = ", velodyne_data_fog.max(axis=0), " ,min = ", velodyne_data_fog.min(axis=0))
```
### Distance Calculation
The Euclidean distances from the origin (0,0,0) to each point are calculated for each dataset. The maximum distance for each dataset is printed.
```python
blickfeld_data_clear_dist = np.sqrt(np.sum(np.square(blickfeld_data_clear), axis=1))
print("blickfeld_data_clear_dist", blickfeld_data_clear_dist.max(axis=0))
radar_data_clear_dist = np.sqrt(np.sum(np.square(radar_data_clear), axis=1))
print("radar_data_clear_dist", radar_data_clear_dist.max(axis=0))
velodyne_data_clear_dist = np.sqrt(np.sum(np.square(velodyne_data_clear), axis=1))
print("velodyne_data_clear_dist", velodyne_data_clear_dist.max(axis=0))

blickfeld_data_fog_dist = np.sqrt(np.sum(np.square(blickfeld_data_fog), axis=1))
print("blickfeld_data_fog_dist", blickfeld_data_fog_dist.max(axis=0))
radar_data_fog_dist = np.sqrt(np.sum(np.square(radar_data_fog), axis=1))
print("radar_data_fog_dist", radar_data_fog_dist.max(axis=0))
velodyne_data_fog_dist = np.sqrt(np.sum(np.square(velodyne_data_fog), axis=1))
print("velodyne_data_fog_dist", velodyne_data_fog_dist.max(axis=0))
```

### Data Shape Verification
The shapes of the distance arrays are printed to verify the calculations
```python
print(blickfeld_data_clear_dist.shape)
print(radar_data_clear_dist.shape)
print(velodyne_data_clear_dist.shape)
print(blickfeld_data_fog_dist.shape)
print(radar_data_fog_dist.shape)
print(velodyne_data_fog_dist.shape)
```

### Plotting Histograms
Histograms of the distance measurements are plotted for each sensor under both clear and foggy conditions. The histograms show both cumulative and non-cumulative distributions to analyze the frequency of different distance measurements
```python
## Blickfeld ##
plt.hist(blickfeld_data_clear_dist, bins=30, color="red", range=[0,30], cumulative=True)
plt.title('Blickfeld - Clear')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

plt.hist(blickfeld_data_clear_dist, bins=30, color="red", range=[0,30], cumulative=False)
plt.title('Blickfeld - Clear')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

plt.hist(blickfeld_data_fog_dist, bins=30, color="blue", range=[0,30], cumulative=True)
plt.title('Blickfeld - Fog')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

plt.hist(blickfeld_data_fog_dist, bins=30, color="blue", range=[0,30], cumulative=False)
plt.title('Blickfeld - Fog')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

## RADAR ##
plt.hist(radar_data_clear_dist, bins=30, color="red", cumulative=True)
plt.title('Radar - Clear')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()
plt.hist(radar_data_clear_dist, bins=30, color="red", cumulative=False)
plt.title('Radar - Clear')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

plt.hist(radar_data_fog_dist, bins=30, color="blue", cumulative=True)
plt.title('Radar - Fog')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()
plt.hist(radar_data_fog_dist, bins=30, color="blue", cumulative=False)
plt.title('Radar - Fog')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

## VELODYNE ##
plt.hist(velodyne_data_clear_dist, bins=50, color="red", range=[0,30], cumulative=True)
plt.title('Velodyne - Clear')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()
plt.hist(velodyne_data_clear_dist, bins=50, color="red", range=[0,30], cumulative=False)
plt.title('Velodyne - Clear')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()

plt.hist(velodyne_data_fog_dist, bins=50, color="blue", range=[0,30], cumulative=True)
plt.title('Velodyne - Fog')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()
plt.hist(velodyne_data_fog_dist, bins=50, color="blue", range=[0,30], cumulative=False)
plt.title('Velodyne - Fog')
plt.xlabel('Distance')
plt.ylabel('Frequency')
plt.show()
```

## Summary
This project analyzes the performance of Blickfeld, radar, and Velodyne sensors in clear and foggy conditions. The Euclidean distances of the x, y, z coordinates are analyzed. Histograms of these distances are plotted to visualize and compare the distance measurements for each sensor under different conditions

## Prerequisites
Python 3.x
pandas
numpy
matplotlib
pathlib

## Usage
Clone the repository.
Update the folder paths to your actual data locations.
Run the script to analyze the data and generate the plots

## License
This project is licensed under the MIT License - see the LICENSE file for details
```bash

Copy and paste this content into a file named `README.md` in your Git repository. This will provide a detailed explanation of your project and guide users on how to use it.

```
