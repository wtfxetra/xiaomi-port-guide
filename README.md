# HyperOS Porting Guide

A practical step-by-step guide for porting HyperOS to your device.  
This guide is written in a simple, hands-on style for modders and ROM builders.  

---

## Prerequisites

Before you start porting HyperOS, make sure you have the following:  

1. ROM of your base device  
   - Must match your device model.  
   - Recommended: use the Xiaomi.eu variant since it comes pre-patched and easier to work with.  

2. ROM you want to port to your device  
   - The HyperOS ROM from the donor device.  
   - Choose one with similar hardware (SoC, GPU, Android version) for better compatibility.  

3. File Manager  
   - Any advanced file manager will work.  
   - MT Manager is highly recommended because it supports APK editing, smali/baksmali, and signing directly on Android.  

4. IMG Tool  
   - To unpack/repack images (`super.img`, `.dat.br`, `.dat`).  
   - Recommended: **DNA Tool**.  

5. Text Editor  
   - For editing `build.prop`, `init.rc`, and config files.  
   - Notepad++, Sublime Text, or even MT Manager’s built-in editor will work.  

6. A Brain  
   - No tool can replace understanding.  
   - Be patient, keep backups, and always compare with a similar working ROM.  

> Tip: Always work on a copy of the ROM and keep a backup of original images.  
> One wrong change in `framework.jar` or `boot.img` can send you into an endless bootloop.  

---

## Preparing Sources

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

### Preparing Images in DNA

1. Open **DNA Tool**.  
2. Select your project (`DNA_Base`).  
3. From the DNA home screen, open **Project Menu**.  

#### Case 0: ZST file present
- If your ROM has `super.img.zst`:  
  1. Open it with **MT Manager**.  
  2. Extract it to get a regular `super.img`.  
  3. Place the extracted `super.img` into your DNA project folder:  
     `Internal storage/DNA/DNA_Base`  
  4. After extraction, you will now have a standard `super.img`.  

#### Case 1: Super.img present
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

#### Case 2: `.br` files present
- If the ROM has files like:  
  - `system.new.dat.br`  
  - `vendor.new.dat.br`  
  - `product.new.dat.br`  
- Use **Unpack BR** in DNA to convert them.  
- After extraction, you will now have a standard `super.img`.  

#### Case 3: `.dat` files present
- If the ROM has files like:  
  - `system.new.dat`  
  - `vendor.new.dat`  
  - `product.new.dat`  
- Use **Unpack DAT** in DNA to convert them.  
- After extraction, you will now have a standard `super.img`.
