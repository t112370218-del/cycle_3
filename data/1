import pandas as pd
import numpy as np

# 1. 讀取資料 (請確保檔名正確)
df = pd.read_csv('YRBS_2007 (1).csv')

# ---------------------------------------------------------
# 第二步：正確處理資料 (Recoding)
# ---------------------------------------------------------

# A. 定義行為變數重編碼函數 (根據文件第 6 節)
# 規則：代碼 2-7 代表「有飲酒」(1)，代碼 1 代表「無飲酒」(0)
def recode_alcohol(code):
    if 2 <= code <= 7:
        return 1  # Success / Exposed Group
    elif code == 1:
        return 0  # Failure / Comparison Group
    else:
        return np.nan # 排除其他無效代碼 (如空白或錯誤填答)

# B. 應用重編碼
df['Alcohol_Group'] = df['CurrentAlcoholUse'].apply(recode_alcohol)

# C. 排除無效值 (重要！)
# 同時過濾掉『組別』或『BMI』有缺少的資料，確保統計結果準確
df_cleaned = df.dropna(subset=['Alcohol_Group', 'BMIPCT']).copy()

# ---------------------------------------------------------
# 檢查處理結果 (描述性統計)
# ---------------------------------------------------------
print("--- 資料處理完成 ---")
# 統計兩組各有多少人
group_counts = df_cleaned['Alcohol_Group'].value_counts().sort_index()
print(f"非飲酒組 (0) 樣本數: {group_counts[0]}")
print(f"飲酒組   (1) 樣本數: {group_counts[1]}")

# 計算兩組的 BMI 平均數與標準差
group_summary = df_cleaned.groupby('Alcohol_Group')['BMIPCT'].agg(['mean', 'std', 'median']).reset_index()
print("\n--- 各組 BMI 百分位摘要 ---")
print(group_summary)

# 計算兩組平均值的原始差異 (Point Estimate of the Difference)
diff = group_summary.loc[1, 'mean'] - group_summary.loc[0, 'mean']
print(f"\n兩組平均值差異 (飲酒組 - 非飲酒組): {diff:.4f}")
