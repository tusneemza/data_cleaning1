# diabetes cleaning

ทำความสะอาดชุดข้อมูล Pima Indians Diabetes ฉบับที่จัดโครงสร้างแล้ว
รายละเอียดทั้งหมด (คำอธิบายคอลัมน์, ขั้นตอนการทำความสะอาด, ผลลัพธ์)
อยู่ใน [README ของ repo](../README.md)

## โครงสร้าง

```
diabetes-cleaning/
├── data/
│   ├── raw/diabetes.csv             # ต้นฉบับ 768 × 9 — ห้ามแก้ไข
│   └── clean/diabetes_cleaned.csv   # ผลลัพธ์ 768 × 12 (ไม่ track ใน git)
├── notebooks/
│   └── cleaning (1).ipynb           # โน้ตบุ๊กทำความสะอาด
├── README.md
└── .gitignore
```

## วิธีรัน

```bash
python -m venv .venv && source .venv/bin/activate
pip install pandas numpy jupyter
jupyter notebook "notebooks/cleaning (1).ipynb"
```

รันทุกเซลล์ตามลำดับ เซลล์สุดท้ายจะเขียน `data/clean/diabetes_cleaned.csv`

## หลักการ

Raw in, clean out — ทุกการแปลงข้อมูลอยู่ในโน้ตบุ๊ก เพื่อให้สร้างไฟล์ที่สะอาดแล้ว
ขึ้นมาใหม่ได้เสมอจาก `data/raw/diabetes.csv` เพียงไฟล์เดียว
