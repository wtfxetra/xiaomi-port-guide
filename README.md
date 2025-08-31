# 📖 Xiaomi VAB Porting Guide

This guide explains how to **port ROMs (including HyperOS) on Xiaomi devices with Virtual A/B (VAB) partitions**. It is intended for developers and advanced users who want to adapt GSI or custom ROMs for their Xiaomi VAB devices.

---

## ⚠️ Disclaimer
- This guide is **for educational purposes only**.
- Flashing custom images always carries the risk of bricking your device.
- Ensure you know how to recover your device using **Fastboot / EDL**.
- I am **not responsible** for any damage or data loss.

---

## ✅ Prerequisites
Before you start porting HyperOS, make sure you have the following:

1. **ROM of your base device**  
   - Must match your device model.  
   - Recommended: use the **Xiaomi.eu variant** since it comes pre-patched and easier to work with.

2. **ROM you want to port to your device**  
   - The **HyperOS ROM** from the donor device.  
   - Choose one with **similar hardware** (SoC, GPU, Android version) for better compatibility.

3. **File Manager**  
   - Any advanced file manager will work.  
   - **MT Manager** is highly recommended because it supports APK editing, smali/baksmali, and signing directly on Android.

4. **IMG Tool**  
   - To unpack/repack images (`super.img`, `.dat.br`, `.dat`).  
   - Recommended: **DNA Tool**.

5. **Text Editor**  
   - For editing `build.prop`, `init.rc`, and config files.  
   - Notepad++, Sublime Text, or even MT Manager’s built-in editor will work.

6. **A Brain**  
   - No tool can replace understanding.  
   - Be patient, keep backups, and always compare with a similar working ROM.

---

## 📂 Preparing Sources

1. Extract the ZIP of your **base ROM** into a folder.  
   - Example: `Base_ROM`  

2. Extract the ZIP of the **ROM you want to port** into a different folder.  
   - Example: `Port_ROM`  

Now you should have two folders:  

- `Base_ROM` → contains the ROM for your device (Xiaomi.eu recommended).  
- `Port_ROM` → contains the HyperOS ROM from the donor device.  

Keep these two folders separate. You will be replacing files from `Port_ROM` into `Base_ROM` during the porting process.  

---

### Creating DNA Projects

Once you have both ROMs extracted, you need to create projects in DNA Tool for easier handling of images.  

1. Open **DNA Tool**.  
2. Select **Create New Project**.  

For each ROM:  

- **Base ROM** → Name the project `Base`  
- **Port ROM** → Name the project `Port`  

DNA will manage files in two locations:  

- **Project storage** → `Internal storage/DNA/DNA_ProjectName`  
- **Unpacked project files** → `/data/DNA/DNA_ProjectName`  

So in this case:  

- Base ROM will be in `Internal storage/DNA/DNA_Base` and `/data/DNA/DNA_Base`  
- Port ROM will be in `Internal storage/DNA/DNA_Port` and `/data/DNA/DNA_Port`

---

## 🖼️ Preparing Images in DNA

1. Open **DNA Tool**.  
2. Select your project (`DNA_Base`).  
3. From the DNA home screen, open **Project Menu**.

### Case 0: ZST file present
- If your ROM has `super.img.zst`:  
  1. Open it with **MT Manager**.  
  2. Extract it to get a regular `super.img`.  
  3. Place the extracted `super.img` into your DNA project folder:  
     `Internal storage/DNA/DNA_Base`  
  4. After extraction, you will now have a standard `super.img`.

### Case 1: Super.img present
- If your ROM has a single `super.img`, continue directly.  
- If your ROM has multiple split supers (e.g. `super.img.0`, `super.img.1`, `super.img.2`):  
  1. Choose **Merge Split Super** in DNA.  
  2. Select all the split files.  
  3. Click **Continue**.  
  4. DNA will create a new `super.img` in:  
     `Internal storage/DNA/DNA_Base/out`  
  5. Move this new `super.img` into your project folder:  
     `Internal storage/DNA/DNA_Base`  
  6. After merging, you will now have a standard `super.img`.

**At last, you will have `super.img` ready. Place it in your base project folder:**  
`Internal storage/DNA/DNA_Base`

---

## 📦 Unpacking Super.img

After placing `super.img` in your base project folder:  

1. Open **DNA Tool**.  
2. Select your project (`DNA_Base`).  
3. Open **Project Menu**.  
4. Select **Unpack Super**.  
5. In the next window, select all partitions shown inside.  
6. Check **Automatically extract images**.  
7. Click **Continue** to unpack all partitions.

---

## 📂 Extracting Port ROM

1. Navigate to your **Port ROM extracted folder**.  
2. The folder will contain either:  
   - `super.img`  
   - Multiple `super.img` files  
   - `.dat` or `.br` files  

3. Use the **same steps as base ROM** to extract:  
   - Merge split `super.img` if needed.  
   - If `.dat` or `.br` files are present, DNA Tool has an option to **extract them** as well.  
4. After extraction, you will have all partitions ready at /data/DNA/projectname

## Porting

1. Copy These Files From Base To Port
   - "product/etc/displayconfig/display_id_xxxxxxxxxx.xml"
   
   - product/overlay/
        - DevicesOverlay.apk
        - DevicesAndroidOverlay.apk
        - AospFrameworkResOverlay.apk
        - MiuiFrameworkResOverlay.apk
        
   - /product/etc/device_features

2. If HOS A15+
   - copy contents from port/mi_ext/etc/build.prop to port/product/etc/build.prop
   
3. If HOS3 A16+
   - Do above step and Remove these props from system / product build.props
   
   - persist.sys.computility.cpulevel=6
   - persist.sys.computility.gpulevel=6
   - persist.sys.computility.version=2025
   - persist.sys.power.cpuinfostorage=true
   - persist.sys.enhance_vkpipelinecache.enable=true
   - persist.sys.power_soc_computing_optimization=true
   
   
4. Done

## Repacking [Dont Mess Up]

1. Pack Product + Mi_ext + System + System_ext of Port

2. Pack Vendor + Odm + Vendor_dlkm of Stock ( Skip if already exist)

3. Most device should be packed as erofs + RO

4. After Pack Place Packed Images in one Project

5. Pack Super.img 
   - 6.5 GB / 8.5 / 9 according to your device
   - VirtualAB
   - RAW
   
6. Place the super.img in a flashing script

## Done
   