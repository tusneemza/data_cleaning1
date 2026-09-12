# data_cleaning1 — Diabetes Data Cleaning

โปรเจกต์ทำความสะอาดข้อมูล (data cleaning) ชุดข้อมูล **Pima Indians Diabetes** ด้วย pandas
ตั้งแต่การสำรวจข้อมูลดิบ จัดการค่าหาย/ค่าที่เป็นไปไม่ได้ จัดการค่าโดด (outliers)
ไปจนถึงการสร้างคอลัมน์เพิ่มเติมและบันทึกไฟล์ที่สะอาดแล้ว

## โครงสร้างโปรเจกต์

```
data_cleaning1/
├── diabetes.csv                 # ข้อมูลดิบ (ชุดแรก, ใช้กับ cleaning.ipynb)
├── cleaning.ipynb               # โน้ตบุ๊กเวอร์ชันแรก — สำรวจ + ทดลองทำความสะอาด
└── diabetes-cleaning/           # เวอร์ชันที่จัดโครงสร้างใหม่ (ใช้ตัวนี้เป็นหลัก)
    ├── data/
    │   ├── raw/diabetes.csv             # ข้อมูลต้นฉบับ — ห้ามแก้ไข
    │   └── clean/diabetes_cleaned.csv   # ผลลัพธ์จากโน้ตบุ๊ก (ไม่ถูก track ใน git)
    ├── notebooks/
    │   └── cleaning (1).ipynb           # ไปป์ไลน์ทำความสะอาดฉบับเต็ม
    ├── README.md
    └── .gitignore
```

## ชุดข้อมูล

ข้อมูลดิบมี **768 แถว × 9 คอลัมน์** และไม่มีแถวซ้ำ

| คอลัมน์ | ความหมาย |
| --- | --- |
| `Pregnancies` | จำนวนครั้งที่ตั้งครรภ์ |
| `Glucose` | ระดับน้ำตาลในพลาสมา (ทดสอบความทนต่อกลูโคส 2 ชม.) |
| `BloodPressure` | ความดันโลหิตค่าล่าง (mm Hg) |
| `SkinThickness` | ความหนาของผิวหนังบริเวณต้นแขน (mm) |
| `Insulin` | อินซูลินในซีรัมที่ 2 ชม. (mu U/ml) |
| `BMI` | ดัชนีมวลกาย (kg/m²) |
| `DiabetesPedigreeFunction` | ค่าประวัติเบาหวานในครอบครัว |
| `Age` | อายุ (ปี) |
| `Outcome` | เป้าหมาย: 1 = เป็นเบาหวาน, 0 = ไม่เป็น |

### ปัญหาที่พบในข้อมูลดิบ

ค่า `0` ในคอลัมน์ทางการแพทย์เป็นไปไม่ได้จริง จึงถือว่าเป็น **ค่าหาย**

| คอลัมน์ | จำนวนค่า 0 | สัดส่วน |
| --- | ---: | ---: |
| `Glucose` | 5 | 0.7% |
| `BloodPressure` | 35 | 4.6% |
| `SkinThickness` | 227 | 29.6% |
| `Insulin` | 374 | 48.7% |
| `BMI` | 11 | 1.4% |

## ขั้นตอนการทำความสะอาด

1. **แปลงค่า 0 เป็น `NaN`** ในคอลัมน์ `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`
2. **เก็บ flag `HasInsulinReading`** (1 = มีผลตรวจอินซูลินจริง, 0 = ถูกเติมทีหลัง)
   เพราะ `Insulin` หายไปเกือบครึ่งของข้อมูล
3. **เติมค่าหายด้วย median แยกตามกลุ่ม `Outcome`** — ใช้ median เพราะทนต่อค่าโดด
   และแยกกลุ่มเพื่อไม่ให้กลุ่มป่วย/ไม่ป่วยดึงค่าของกันและกัน
4. **จัดการค่าโดดด้วย IQR capping** (clip ที่ `Q1 − 1.5·IQR` และ `Q3 + 1.5·IQR`)
   กับคอลัมน์ตัวเลขทั้ง 8 คอลัมน์
5. **ลบแถวซ้ำ** ด้วย `drop_duplicates()` (ชุดนี้ไม่มีแถวซ้ำ แต่ทำไว้เป็นมาตรฐาน)
6. **สร้างคอลัมน์เพิ่ม (feature engineering)**
   - `Outcome_Label` — `Negative` / `Positive`
   - `AgeGroup` — แบ่งช่วงอายุเป็น `20s`, `30s`, `40s`, `50s`, `60+`
7. **บันทึกผลลัพธ์** ไปที่ `data/clean/diabetes_cleaned.csv`

## ผลลัพธ์

`data/clean/diabetes_cleaned.csv` — **768 แถว × 12 คอลัมน์** ไม่มีค่าหายเหลืออยู่
(9 คอลัมน์เดิม + `HasInsulinReading`, `Outcome_Label`, `AgeGroup`)

การกระจายตามช่วงอายุ: 20s = 396, 30s = 165, 40s = 118, 50s = 57, 60+ = 32

## วิธีใช้งาน

```bash
python -m venv .venv && source .venv/bin/activate
pip install pandas numpy jupyter

cd diabetes-cleaning
jupyter notebook "notebooks/cleaning (1).ipynb"
```

รันทุกเซลล์ตามลำดับ เซลล์สุดท้ายจะเขียนไฟล์ `data/clean/diabetes_cleaned.csv` ให้อัตโนมัติ

## หลักการทำงาน

**Raw in, clean out** — ไฟล์ใน `data/raw/` เป็นแหล่งข้อมูลจริงที่ไม่แก้ไขเด็ดขาด
ทุกการแปลงข้อมูลอยู่ในโน้ตบุ๊ก เพื่อให้สร้างไฟล์ที่สะอาดแล้วขึ้นมาใหม่ได้เสมอ
จาก `data/raw/diabetes.csv` เพียงไฟล์เดียว
