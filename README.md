# Radix Sort OpenMPI

## Petunjuk Penggunaan Program
Jalankan ini dalam terminal :
**cd openmpi/src**
**make**
**mpirun -np 4 radix_sort (N element array)

##Pembagian Tugas
Thareq Muhammad Yusuf Hasnul Aziz - 13516004 :
 - Parallel radix sort dengan OpenMPI
 
 Sinaga Yoko Christoffel Triandi - 13516052 : 
 - Laporan
 
 ## Laporan Pengerjaan
 
 ### Deskripsi Solusi Paralel
 Solusi yang digunakan adalah dengan menggunakan MPI_Scatter dan MPI_Gather dalam Radix Sort. 
 MPI_Scatter digunakan untuk menyebar data pada suatu task. MPI_Scatter diimplementasikan untuk algoritma
 pemberian *flag* pada array acak. Setelah itu, data dari tiap task akan diambil kembali menggunakan
 MPI_Gather. MPI_Gather akan dilakukan setelah program selesai menjalankan algoritma pemberian *flag*.
 
 ### Analisis Solusi
 Menurut kami, penerapan MPI pada *Radix Sort* dapat membantu mempercepat sort. Namun karena MPI_Scatter dan
 MPI_Gather membutuhkan waktu yang cukup lama untuk membagi task dan mengumpulkannya, proses *Radix Sort*
 menjadi relatif sedikit lebih lambat.
 
 ### Jumlah Thread
 Jumlah Thread yang digunakan adalah 5000, 50000, 100000, 200000, dan 400000, dan hasilnya adalah sebagai berikut:
 
 #### Thread 5000 :
 Waktu Radix Sort Serial = microsecond
 Waktu Radix Sort Parallel = microsecond
 
  #### Thread 50000 :
 Waktu Radix Sort Serial = microsecond
 Waktu Radix Sort Parallel = microsecond
 
  #### Thread 100000 :
 Waktu Radix Sort Serial = microsecond
 Waktu Radix Sort Parallel = microsecond
 
  #### Thread 200000 :
 Waktu Radix Sort Serial = microsecond
 Waktu Radix Sort Parallel = microsecond
 
  #### Thread 400000 :
 Waktu Radix Sort Serial = microsecond
 Waktu Radix Sort Parallel = microsecond
 
 ###Analisis Perbandingan Kinerja Serial dan Paralel
 Dari hasil tes uji yang telah dilakukan, didapatkan hasil bahwa waktu yang dibutuhkan saat serial lebih 
 cepat dibanding waktu yang dibutuhkan saat paralel . Namun, semakin besar banyak thread, waktu yang dibutuhkan 
 saat parallel semakin lebih cepat dibanding dengan waktu yang dibutuhkan saat serial.
