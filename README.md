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
