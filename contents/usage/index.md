# **SLURM**

Trước khi bắt đầu, bạn nên nắm một số kiến thức cơ bản về Linux. 

[Nguồn tài liệu học sử dụng Linux](https://www.notion.so/Ngu-n-t-i-li-u-h-c-s-d-ng-Linux-5886beccfb4d4907a0fcd1b4ee20eb00?pvs=21)

# **Giới thiệu Slurm**

Trên tất cả các hệ thống, người dùng chạy chương trình bằng cách gửi script tới Slurm. Slurm script phải thực hiện ba việc:

1. Quy định các yêu cầu về tài nguyên cho job
2. cài đặt môi trường
3. Chỉ định job sẽ được thực hiện dưới dạng lệnh shell

Dưới đây là tập lệnh Slurm mẫu để chạy mã Python bằng môi trường Conda:

```
#!/bin/bash
#SBATCH --job-name=myjob         # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=2G         # memory per cpu-core (4G is default)
#SBATCH --time=00:01:00          # total run time limit (HH:MM:SS)

module purge
module load anaconda3/2023.9
conda activate pytools-env

python myscript.py
```

- Dòng đầu tiên của tập lệnh Slurm chỉ định shell Unix sẽ được sử dụng.
- Tiếp theo là một loạt lệnh #SBATCH đặt ra các yêu cầu về tài nguyên và các tham số khác của job.
- Tập lệnh trên yêu cầu 1 lõi CPU và 4 GB bộ nhớ trong thời gian chạy 1 phút. Những thay đổi cần thiết đối với môi trường được thực hiện bằng cách tải module môi trường anaconda3/ và kích hoạt một môi trường Conda cụ thể.
- Cuối cùng, job cần thực hiện, tức là thực thi tập lệnh Python, được chỉ định ở dòng cuối cùng.

Xem bên dưới để biết thông tin về sự tương ứng giữa các tác vụ và lõi CPU. Nếu job của bạn không hoàn thành trước thời hạn quy định thì nó sẽ bị hủy. Bạn nên sử dụng giá trị chính xác cho thời gian ước lượng nhưng thêm thêm 20% để đảm bảo an toàn.

Một tập lệnh job có tên `job.slurm` được gửi tới bộ lập lịch Slurm bằng lệnh `sbatch`:

```
$ sbatch job.slurm
```

Job phải được gửi đến từ head node của một cluster. Job scheduler sẽ sắp xếp job ở nơi nó sẽ giữ nguyên cho đến khi nó có đủ mức độ ưu tiên để chạy trên compute node. Tùy thuộc vào tính chất job và nguồn lực sẵn có, thời gian xếp hàng sẽ thay đổi từ vài giây đến nhiều ngày. Để kiểm tra trạng thái của các job được xếp hàng đợi và đang chạy, hãy sử dụng lệnh sau:

```
$ squeue -u <username>
```

Để xem thời gian bắt đầu dự kiến của các job của bạn:

```
$ squeue -u <username> --start
```

## **Các câu lệnh Slurm thường dùng**

### **Tìm hiểu trước về hệ thống**

| Lệnh | Mô tả |
| --- | --- |
| checkquota (chưa hoạt động) | xem việc sử dụng dung lượng ổ đĩa của bạn, kiểm tra xem bạn có đủ dung lượng trong các thư mục của mình không, liên kết ở phía dưới để yêu cầu thêm dung lượng |
| cat /etc/os-release | thông tin về hệ điều hành |
| lscpu | thông tin về CPU trên nút cụ thể đó |
| snodes | thông tin về các nút tính toán |
| shownodes | thông tin về các nút tính toán (dễ đọc hơn) |
| sinfo | xem tất cả các nút của cụm đang được sử dụng như thế nào (ví dụ: chúng có ở trạng thái rảnh không? ngừng hoạt động? đang được sử dụng?) |
| sininfo -p gpu | xem GPU đang được sử dụng như thế nào |
| qos | "quality of service" - xem cách phân chia job và giới hạn trên mỗi phân vùng |
| top | hiển thị hoạt động của bộ xử lý, các lệnh đang chạy (nhấn 'q' để thoát) |
| htop | hiển thị hoạt động của bộ xử lý, các lệnh đang chạy, có màu sắc (nhấn 'q' để thoát) |
| sshare | hiển thị các chia sẻ cụm được chỉ định bởi nhóm |
| sprio -w | xem mức độ ưu tiên job được chỉ định như thế nào (hiển thị trọng số cho từng yếu tố) |

### **Gửi một Job**

| Lệnh | Mô tả |
| --- | --- |
| sbatch | gửi job của bạn tới job scheduler |
| srun | yêu cầu một interactive job trên computing node (xem bên dưới) |

### **Sau khi Job được submit**

| Yêu cầu | Sự miêu tả |
| --- | --- |
| squeue | hiển thị tất cả các job đang chạy hoặc chờ chạy |
| squeue -u <username> | xem job của bạn đang chạy hoặc đang chờ chạy |
| squeue --start | báo cáo thời gian bắt đầu dự kiến ​​cho các job đang chờ xử lý |
| squeue -j <jobid> | hiển thị các nút đang được sử dụng cho job đang chạy của bạn |
| scontrol show jobid <jobid> | hiển thị thông tin chi tiết về job |
| sprio | hiển thị mức độ ưu tiên được giao cho các job đang chờ xử lý |
| slurmtop | hiển thị job hiện tại |
| scancel | hủy job (ví dụ: scancel 2534640) |

### **Sau khi job hoàn thành**

| Lệnh | Mô tả |
| --- | --- |
| seff <jobid> | xem kết quả job đã hoàn thành |
| shistory | hiển thị lịch sử job của bạn |
| jobstats <jobid> | xem số liệu chính xác về bộ nhớ, CPU và GPU từ các job. Xem thêm về số liệu thống kê job. |

## **Your First Slurm Job**

Nếu bạn chưa quen với cụm HPC hoặc Slurm thì hãy xem hướng dẫn này để chạy job đầu tiên của bạn. 

## **Terminology**

Khi bạn SSH đến một cụm, bạn đang kết nối với head node, node này được tất cả người dùng chia sẻ. Việc chạy job trên nút đăng nhập bị **CẤM**. Các job hàng loạt và tương tác phải được gửi từ head node đến bộ lập lịch job của Slurm bằng cách sử dụng lệnh “srun”, "sbatch" và "salloc". Sau khi xếp hàng đợi, các job sẽ được gửi đến các compute node thực hiện job tính toán thực tế.

Mỗi CPU có nhiều lõi CPU. Chạy lệnh "snodes" và xem cột "CPUS" ở đầu ra để xem số lượng lõi CPU trên mỗi nút cho một cụm nhất định. Bạn sẽ thấy các giá trị như 28, 32, 40, 96 và 128. Nếu job của bạn yêu cầu số lượng lõi CPU trên mỗi nút trở xuống thì hầu như bạn luôn nên sử dụng --nodes=1 trong tập lệnh Slurm của mình. Các giá trị cho --ntasks và --cpus-per-task được giải thích rõ nhất bằng các ví dụ bên dưới. 

Lệnh --time đặt thời gian chạy tối đa cần thiết cho job của bạn. Bạn nên đặt giá trị này một cách chính xác nhưng hãy tính thêm 20% vì job sẽ bị hủy nếu nó không hoàn thành trước khi đạt đến giới hạn. Để đặt yêu cầu bộ nhớ của job bằng cách sử dụng --mem-per-cpu hoặc --mem.

## **Interactive Allocations with salloc**

Nút đăng nhập của cụm chỉ có thể được sử dụng cho job tương tác rất nhẹ sử dụng tối đa 10% máy (lõi CPU và bộ nhớ) trong tối đa 10 phút. Điều này được thực thi nghiêm ngặt vì việc vi phạm quy tắc này thường có thể ảnh hưởng xấu đến job của những người dùng khác. Job tương tác chuyên sâu phải được thực hiện trên các nút điện toán bằng lệnh salloc. Để hoạt động tương tác trên nút điện toán có 1 lõi CPU và 4 GB bộ nhớ trong 20 phút, hãy sử dụng lệnh sau:

```
$ salloc --nodes=1 --ntasks=1 --mem=4G --time=00:20:00
```

Giống như các job hàng loạt, việc phân bổ tương tác sẽ đi qua hệ thống xếp hàng. Điều này có nghĩa là khi clusterr bận, bạn sẽ phải đợi trước khi được cấp phân bổ. Bạn sẽ thấy đầu ra như sau:

```
salloc: Pending job allocation 32280311
salloc: job 32280311 queued and waiting for resources
salloc: job 32280311 has been allocated resources
salloc: Granted job allocation 32280311
salloc: Waiting for resource configuration
salloc: Nodes della-r4c1n13 are ready for job
[aturing@della-r4c1n13 ~]$
```

Sau thời gian chờ đợi, bạn sẽ được đưa vào một shell trên compute node nơi bạn có thể bắt đầu làm việc một cách tương tác. Lưu ý rằng việc phân bổ của bạn sẽ chấm dứt khi đạt đến giới hạn thời gian. Bạn có thể sử dụng lệnh thoát để kết thúc phiên và quay lại nút đăng nhập bất cứ lúc nào.

**GPU**

Để yêu cầu một nút có GPU:

```
$ salloc --nodes=1 --ntasks=1 --mem=4G --time=00:20:00 --gres=gpu:1 
```

**Graphics**

Nếu bạn đang làm việc với đồ họa thì hãy bật chuyển tiếp X11 (và xem các yêu cầu bổ sung):

```
$ salloc --nodes=1 --ntasks=1 --mem=4G --time=00:20:00 --x11
```

## **GPU Job**

Xem số lượng GPU trên mỗi cluster tại trang thông tin cluster đó. Để sử dụng GPU trong job, hãy thêm câu lệnh SBATCH với tùy chọn --gres:

```
#!/bin/bash
#SBATCH --job-name=mnist         # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=4G         # memory per cpu-core (4G is default)
#SBATCH --gres=gpu:1             # number of gpus per node
#SBATCH --time=00:01:00          # total run time limit (HH:MM:SS)

module purge
module load anaconda3/2023.3
conda activate tf2-gpu

python myscript.py
```

Ví dụ: để sử dụng bốn GPU trên mỗi nút, dòng thích hợp sẽ là:

```
#SBATCH --gres=gpu:4
```

QUAN TRỌNG: Chỉ những mã được viết rõ ràng để chạy trên GPU mới có thể tận dụng GPU. Việc thêm tùy chọn --gres vào tập lệnh Slurm cho mã chỉ dành cho CPU sẽ không tăng tốc thời gian thực thi nhưng sẽ lãng phí tài nguyên, tăng thời gian xếp hàng và giảm mức độ ưu tiên của lần gửi job tiếp theo. Hơn nữa, một số mã chỉ được viết để sử dụng một GPU duy nhất nên tránh yêu cầu nhiều GPU trừ khi mã của bạn có thể sử dụng chúng. Nếu mã có thể sử dụng nhiều GPU thì bạn nên tiến hành phân tích tỷ lệ để tìm ra số lượng GPU tối ưu để sử dụng.

Xem [thông tin hệ thống](../introduce-to-cluster/) để biết về cấu hình của server. Lưu ý rằng một số mã yêu cầu sử dụng nhiều lõi CPU cùng với GPU để có hiệu suất tối ưu.

## **Job Arrays**

Mảng job được dùng để chạy cùng một job nhiều lần mà chỉ có những khác biệt nhỏ giữa các job. Ví dụ: giả sử bạn cần chạy 100 job, mỗi job có một giá trị hạt giống khác nhau cho trình tạo số ngẫu nhiên. Hoặc có thể bạn muốn chạy cùng một tập lệnh phân tích dữ liệu cho từng file trong tổng số 50 files. Job array là lựa chọn tốt nhất cho những trường hợp như vậy.

Dưới đây là một ví dụ về tập lệnh Slurm để chạy Python trong đó có 5 job:

```
#!/bin/bash
#SBATCH --job-name=array-job     # create a short name for your job
#SBATCH --output=slurm-%A.%a.out # stdout file
#SBATCH --error=slurm-%A.%a.err  # stderr file
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=4G         # memory per cpu-core (4G is default)
#SBATCH --time=00:01:00          # total run time limit (HH:MM:SS)
#SBATCH --array=0-4              # job array with index values 0, 1, 2, 3, 4

echo "My SLURM_ARRAY_JOB_ID is $SLURM_ARRAY_JOB_ID."
echo "My SLURM_ARRAY_TASK_ID is $SLURM_ARRAY_TASK_ID"
echo "Executing on the machine:" $(hostname)

module purge
module load anaconda3/2023.3
conda activate myenv

python myscript.py
```

Dòng chính trong tập lệnh Slurm ở trên là:

```
#SBATCH --array=0-4
```

Trong ví dụ này, tập lệnh Slurm sẽ chạy năm job. Mỗi job sẽ có giá trị SLURM_ARRAY_TASK_ID khác nhau (tức là 0, 1, 2, 3, 4). Giá trị của SLURM_ARRAY_TASK_ID có thể được sử dụng để phân biệt các job trong mảng. Xem ví dụ đầy đủ về Python. Người ta có thể chuyển SLURM_ARRAY_TASK_ID cho tệp thực thi dưới dạng tham số dòng lệnh hoặc tham chiếu nó dưới dạng biến môi trường. Sử dụng cách tiếp cận thứ hai, một vài dòng đầu tiên của tập lệnh Python (được gọi là myscript.py ở trên) có thể trông như thế này:

```
import os
idx = int(os.environ["SLURM_ARRAY_TASK_ID"])
parameters = [2.5, 5.0, 7.5, 10.0, 12.5]
myparam = parameters[idx]
# execute the rest of the script using myparam
```

## Các job sử dụng bộ nhớ lớn

Một lợi thế của việc sử dụng cụm HPC trên máy tính xách tay hoặc máy trạm của bạn là lượng RAM lớn có sẵn trên mỗi nút. Ví dụ: trên một số cụm, bạn có thể chạy một job với bộ nhớ 100 GB. Điều này có thể rất hữu ích khi làm việc với một tập dữ liệu lớn. Để tìm hiểu xem mỗi nút có bao nhiêu bộ nhớ, hãy chạy lệnh snodes và xem cột MEMORY tính bằng megabyte.

```
#!/bin/bash
#SBATCH --job-name=slurm-test    # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem=100G               # memory per node (4G per cpu-core is default)
#SBATCH --time=00:01:00          # total run time limit (HH:MM:SS)

module purge
module load anaconda3/2023.3
conda activate micro-env

python myscript.py
```

Ví dụ trên chạy tập lệnh Python sử dụng 1 lõi CPU và 100 GB bộ nhớ. Trong tất cả các tập lệnh Slurm, bạn nên sử dụng giá trị chính xác cho bộ nhớ cần thiết nhưng thêm 20% để đảm bảo an toàn. Để biết thêm, hãy xem Phân bổ bộ nhớ.

## **Báo cáo (hiện tại chưa hỗ trợ)**

Nếu bạn đưa chỉ thị thư SBATCH bên dưới vào tập lệnh Slurm thì bạn sẽ nhận được email sau khi mỗi job kết thúc:

```
#SBATCH --mail-type=end
#SBATCH --mail-user=<yourEmail>
```

Dưới đây là một báo cáo mẫu:

```
================================================================================
                              Slurm Job Statistics
================================================================================
         Job ID: 1234567
  NetID/Account: aturing/math
       Job Name: sys_logic_ordinals.sh
          State: COMPLETED
          Nodes: 1
      CPU Cores: 1
     CPU Memory: 64GB
           GPUs: 1
  QOS/Partition: gpu-short/gpu
        Cluster: della
     Start Time: Thu May 11, 2023 at 1:52 PM
       Run Time: 14:27:09
     Time Limit: 16:00:00

                              Overall Utilization
================================================================================
  CPU utilization  [|||||||||||||||||||||||||||||||||||||||||||||| 93%]
  CPU memory usage [|||||||||||||||                                30%]
  GPU utilization  [||||||||||||||||||||||||||||||||||||||||||     85%]
  GPU memory usage [|||||||||||||||||||||||||||||||||||||||||||||| 92%]

                              Detailed Utilization
================================================================================
  CPU utilization per node (CPU time used/run time)
      della-l05g2: 00:25:16/00:27:09 (efficiency=93.1%)

  CPU memory usage per node - used/allocated
      della-l05g2: 19.3GB/64.0GB (19.3GB/64.0GB per core of 1)

  GPU utilization per node
      della-l05g2 (GPU 1): 85.3%

  GPU memory usage per node - maximum used/total
      della-l05g2 (GPU 1): 73.6GB/80.0GB (92.0%)

                                     Notes
================================================================================
```

Báo cáo cung cấp thông tin về thời gian chạy, mức sử dụng CPU, mức sử dụng bộ nhớ, v.v. Bạn nên kiểm tra các giá trị này để xác định xem bạn có đang sử dụng tài nguyên đúng cách hay không. Xem trang Thống kê job để biết thêm thông tin.

Thời gian xếp hàng của bạn một phần được xác định bởi lượng tài nguyên bạn yêu cầu. Giá trị chia sẻ công bằng của bạn, một phần quyết định mức độ ưu tiên của job tiếp theo của bạn, sẽ giảm tỷ lệ với lượng tài nguyên bạn đã sử dụng trong 30 ngày trước đó.

## **Tài liệu đọc thêm**

**[external resources for learning Slurm](https://researchcomputing.princeton.edu/education/external-online-resources/slurm)**.