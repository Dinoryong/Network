# Computer Memory

------

> 
>
> [toc]



## 1. Retrieving Data From A Computer

> modern computer architecture values : data capacity, access speed, volatility, read-only or read-write

SSD

- store permanent data

ROM disks - read-only, cannot be changed or overwritten

RAM disks - read-write, can be changed or overwritten

HDD

- (A) read 200 files, 50 \text{ kB}50 kB each (10 \text{ MB}10 MB total)
- (B) read 20 files, 500 \text{ kB}500 kB each (10 \text{ MB}10 MB total)

- B is 7 times faster than A 
  - A = 0.01 + 50/100000 = 0.0105s , 200 files * 0.0105 = 2.1 s 
  - B = 0.01 + 50/100000 = 0.015s , 20 files * 0.015 = 0.3 s
- non-volatile
- stroe permanent data

RAM - semiconductor module, stores daa within an integrated circuit, 100 times faster tan HDDs

- `volatile` memory - only hold data when power is provided, lose data when power is removed.
- store intermediate data, to calculate
- accessed frequently from programs => also called `main memory`

Programs will load files from the HDD or SSD to RAM

- Given RAM and HDD where the access speed is 10 GB/s and 100 MB/s respectively, we want to read a 3MB file 25 times. Ignoring random access overhead and the cost of writing to RAM, which answer is correct regarding the speed of (A) and (B)?

  (A) load the file from the HDD to RAM once, and read from RAM 25 times.

  (B) read directly from the HDD 25 times.
  - 10 GB/s is equivalent to 10000 \text{ MB/s}10000 MB/s.

    Loading a 3\text{ MB}3 MB file once from the 100 \text{ MB/s}100 MB/s HDD takes \dfrac{3 }{ 100} = 0.03 \text{ s}1003=0.03 s, whereas
    loading a 3\text{ MB}3 MB file once from the 10000 \text{ MB/s}10000 MB/s RAM takes \dfrac3 { 10000 }= 0.0003 \text{ s}100003​=0.0003 s.

    (A) takes 1 \times 0.03 + 25 \times 0.0003 = 0.0375 \text{ s}1×0.03+25×0.0003=0.0375 s, while
    (B) takes 25 \times 0.03 = 0.75 \text{ s}25×0.03=0.75 s.

    \dfrac{0.75 }{ 0.0375 } = 200.03750.75=20. Therefore, (A) is 20 times faster than (B).











































