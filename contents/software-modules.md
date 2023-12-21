---
label: Software modules
order: 0
---

# **Cách sử dụng Software Modules** (TODO: thay bằng spack)

Một số lượng lớn các phần mềm được cài đặt trên các cụm HPC. Để quản lí chúng, ta sử dụng software modules. Một số phần mềm được cung cấp thông qua các module chung. Sau khi đọc xong ta có thể

- sử dụng "module avail” để liệt kê các mô-đun có sẵn
- load và unload modules
- sử dụng "module show” để xem module thay đổi môi trường của bạn như thế nào

## **Available Modules**

Để xem các module có sẵn, sử dụng lệnh **module avail**:

```
$ module avail

---------- /usr/local/share/Modules/modulefiles -----------------
boost/1.55.0
boost/openmpi-1.10.2/1.55.0
cudann/cuda-8.0/5.0
cudann/cuda-8.0/5.1
cudatoolkit/10.0
...
-------------- /opt/share/Modules/modulefiles --------------
intel/16.0/64/16.0.4.258
intel/17.0/64/17.0.5.239
intel/18.0/64/18.0.3.222
...

```

Hoặc xem các module cụ thể:

```
$ module avail anaconda3

--------- /usr/licensed/Modules/modulefiles ----------
anaconda3/2018.12  anaconda3/2020.7   anaconda3/2022.5
anaconda3/2019.3   anaconda3/2020.11  anaconda3/2022.10
anaconda3/2019.10  anaconda3/2021.5   anaconda3/2023.3
anaconda3/2020.2   anaconda3/2021.11  anaconda3/2023.9

```

## **Loading Modules**

Để đảm bảo reproducibility, khi load tên module sử dụng toàn bộ tên module/ đường dẫn**. Ví dụ**

`$ module load intel/19.0/64/19.0.3.199 openmpi/intel-17.0/3.1.3/64`

Hoặc

`$ module load intel/19.0/64/19.0.3.199`

`$ module load openmpi/intel-17.0/3.1.3/64`

Ở một số cluster, có thể sẽ không tìm thấy module yêu cầu

```
$ module load anaconda3
ERROR: No default version defined for 'anaconda3'

$ module load anaconda3/2023.9
$ python --version
3.11.5

```

Để kiểm tra các module đã load, sử dụng **module list**:

```jsx
$ module list
Currently Loaded Modulefiles:
1) openmpi/intel-17.0/3.1.3/64
2) intel-mkl/2019.3/3/64
3) intel/19.0/64/19.0.3.199
4) anaconda3/2023.9
```

Lưu ý không sử dụng **module load *modulename/version*** bên trong startup script (e.g., ~/.bashrc) vì nó có thể dẫn đến xung đột và các sự cố khó chẩn đoán. Thay vào đó, hãy tải các module trực tiếp vào Slurm script của bạn.

## **Module Aliases**

Nếu bạn muốn sử dụng các tên ngắn thay vì tên module đầy đủ thì hãy tạo tệp `~/.modulerc` như sau::

```
cat ~/.modulerc
#%Modulerc
module-alias anaconda3 anaconda3/2023.9
module-alias myintel intel/19.1.1.217
```

Khi đó bạn có thể:

```
$ module load anaconda3 myintel
```

## **Load module thay đổi môi trường làm việc của bạn thế nào?**

Để xem việc load module thay đổi gì môi trường của bạn, chạy lệnh **module show *modulename/version***. Ví dụ:

```
$ module show gsl/2.6
-------------------------------------------------------------------
/usr/local/share/Modules/modulefiles/gsl/2.6:

module-whatis	 Sets up gsl26 2.6 in your environment
setenv		 GSL_ROOT_DIR /usr/local/gsl/2.6/x86_64
prepend-path	 PATH /usr/local/gsl/2.6/x86_64/bin
prepend-path	 CPATH /usr/local/gsl/2.6/x86_64/include
prepend-path	 LD_LIBRARY_PATH /usr/local/gsl/2.6/x86_64/lib64
prepend-path	 LIBRARY_PATH /usr/local/gsl/2.6/x86_64/lib64
prepend-path	 MANPATH /usr/local/gsl/2.6/x86_64/share/man
append-path	 -d   LOCAL_CFLAGS -I/usr/local/gsl/2.6/x86_64/include
prepend-path	 PKG_CONFIG_PATH /usr/local/gsl/2.6/x86_64/lib64/pkgconfig
-------------------------------------------------------------------

```

Những điều này hữu ích khi bạn chỉnh sửa Makefiles.

## **Xoá và gỡ *Modules***

Để xoá toàn bộ modules, sử dụng

```
$ module purge
```

Lệnh trên thường được sử dụng trong các Slurm script để đảm bảo môi trường sạch sẽ trước khi thiết lập môi trường cho công việc cụ thể.

Để gỡ một module khỏi môi trường, sử dụng **module unload**. Ví dụ

```
$ module unload anaconda3/2023.9
```

Để biết thêm về module, sử dụng `module help`

# **Cài đặt module chưa có trong cluster (WIP)**

Để cài đặt các module không có trong cluster, bạn xem qua link A và B để xem các module có thể cài đặt. Nếu tồn tại module đó, bạn có thể yêu cầu quản trị viên cài đặt thông qua form bên dưới, hoặc bạn có thể tạo một image đã cài sẵn phần mềm đó để có quyền tuỳ chỉnh và cả quyền root, đồng thời rất tốt cho việc reproduce kết quả. Tuy nhiên việc tạo Docker images trong lúc phát triển phần mềm yêu cầu một lượng lớn dung lượng, hãy cân nhắc.

## **Yêu cầu Module**

TODO: 

## **Sử dụng docker**

tham khảo thêm tại [Containers on the Research Computing Clusters | Princeton Research Computing](https://researchcomputing.princeton.edu/support/knowledge-base/apptainer)