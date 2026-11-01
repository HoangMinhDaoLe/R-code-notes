# R-code-notes

getwd                                      #to identify file path
setwd "your/path/to/the/file"             #this is to set new file path

install.packages("readxl")               #If needing excel file but couldn't upload it
library(readxl)                            #Check the package for EXCEL FILE

df_stata <- read_dta("duong_dan_den_file/ten_file.dta")   #Adding stata file 

#But before that, make sure "haven" is installed as a package in R.



   #This gtsummary would provide better visualisation for research report or thesis and poster presentation.
library(gtsummary)

df %>%
  select(gioi_tinh, bien_ket_qua) %>%
  tbl_summary(
    by = gioi_tinh, # Đưa giới tính ra làm cột ngang
    statistic = list(all_categorical() ~ "{n} ({p}%)") # Hiển thị Số lượng (Tỷ lệ %)
  ) %>%
  add_p() # Thêm cột p-value so sánh giữa Nam và Nữ


#Conventional way:
#To have descriptive statistics (Table) with (Male and female in the horizontal row) and the outcome value in the vertical row
# Bước 1: Lập bảng tần số (Hàng: Yes/No, Cột: Nam/Nữ)
bang_dem <- table(df$bien_ket_qua, df$gioi_tinh)        #Replace by any correct variable to fill in this code
print(bang_dem)

# Bước 2: Tính tỷ lệ phần trăm theo CỘT (margin = 2)
# Ý nghĩa: Tổng mỗi cột (Nam hoặc Nữ) sẽ là 100%, xem trong đám Nam thì bao nhiêu % Yes/No, và Nữ là bao nhiêu %
bang_ty_le <- prop.table(bang_dem, margin = 2) * 100
print(round(bang_ty_le, 2)) # Làm tròn 2 chữ số thập phân cho đẹp    



#To include percentages, use janitor (tabyl, adorn_percentages, adorn_pct_formatting, adorn_ns)
library(dplyr)
library(janitor)

df %>% 
  tabyl(bien_ket_qua, gioi_tinh) %>%               # 1. Lập bảng (Hàng: biến đầu ra, Cột: giới tính)
  adorn_percentages("col") %>%                     # 2. Tự động tính % theo từng Cột (Nam/Nữ)
  adorn_pct_formatting(digits = 1) %>%             # 3. Định dạng lấy 1 chữ số thập phân (VD: 40.0%)
  adorn_ns(position = "combined")                  # 4. Tự động gộp số lượng vào trong ngoặc: Số_lượng (Tỷ_lệ%)


      
