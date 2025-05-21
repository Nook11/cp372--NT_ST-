# 📊 Nike Global Sales Analysis (2024)
วิเคราะห์ยอดขายสินค้า Nike ปี 2024 โดยเน้นการเพิ่มยอดขายผ่านการสำรวจเชิงลึกด้วย Tableau การคำนวณกำไร และการวิเคราะห์ข้อมูล
## Data Set
Nike Global Sales ข้อมูลปี 2024 - [Data Set Download](./Data_Set_nike_sales_2024.xlsx)
## Problem Statement
* Nike จะมีการจำหน่ายสินค้าในหลากหลายประเทศ เเละหลากหลายประเภท เเต่สินค้าในหมวดหมู่Footwear ยอดขายที่ต่ำมากอย่างเห็นได้ชัด ซึ่งควรจะต้องเป็นสินค้าหลักเมื่อเทียบกับหมวดอื่นๆ
## Business Objectives
* การเพิ่มยอดขายสินค้าหมวดหมู่Footwaerเพิ่มขึ้นอย่างน้อย 5% ภายใน6เดือน โดยศึกษาจากยอดขายของปี 2024
## Key Stakeholders
* ฝ่ายการตลาดและฝ่ายขาย
## Success Metrics
* การเติบโตของเเต่ละไตรมาส
* ยอดขายของเเต่ละเดือน
# การเตรียมข้อมูล (Data Preparation) 
#### 1.การทำความสะอาดข้อมูล (Data Cleaning) :
ขั้นตอน: โหลดข้อมูล: ใช้ pandas.read_csv() เพื่ออ่านไฟล์  nike_sales_2024_pj.csv
* ตวรจสอบค่าที่ว่าง
```python
df.isnull().sum()
```
* การลบข้อมูลซ้ำ
```python
df = df.drop_duplicates()
```
