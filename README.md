###1. Block

- Block đơn giản chỉ là tập hợp các lệnh thành một khối
- Được đặt trong dấu {...}
- Trong ruby thì block phổ biến và dễ dùng hơn so với lambda và proc
- Ví dụ 1 block
```
array = [1, 2, 3]
array.collect! do |n|
  n + 1
end
puts array.inspect
# => [2, 3, 4]
or 
[1, 2, 3].each {|x| puts x}
```

- Khi sử dụng block thì có hạn chế đó là khi ta muốn thay đổi đầu vào thì ta phải viết lại toàn bộ block để hiển thị giá trị cho input mới

###2. Proc

- Cấu trúc của block là đơn giản tiện dụng dễ dùng, nhưng khi ta thay đổi input thì ta lại phải viết block mới, việc này dẫn tới trùng lặp code, vì vậy ta có Proc để giải quyết vấn đề này
- 1 proc thực chất là 1block được đặt tên
- Ví dụ
```
proc = Proc.new {|x| puts x + 1}
```
- Gọi proc

  Đối với input truyền vào là 1 mảng 
  ```
  [1, 2, 3].each(&proc)
  ```
  Với 1 giá trị có thể gọi 
  ```
  proc.call(2)
  ```
  Khi muốn thay đổi input thì ta gọi lại với giá trị mới như sau
  ```
  [2, 5, 6].each(&proc)
  ```
  Ký & để hiểu tham số truyền vào là 1proc
- 1 proc là một object

###3. Lambda

- Lambda là một function và không có tên cụ thể
- Nó có thể được sử dụng để gán 1 đoạn code
- Là 1 object
- Return về 1 value như các function khác
- Về cách viết lambda
  - Mọi người có thể xem các ví dụ sau thì sẽ hình dung ra được các viết 1 lambda
  ```
  result = lambda { |x| x + 1 }
  puts result.call(10)
  result = ->(x) { x + 1 }
  puts result.call(10)
  ```
  - Ruby dùng { } để viết lambda với 1 dòng code và dùng do end để viết một lambda với nhiều dòng code
  ```
  result =  lambda do |phuongthuc|
    if phuongthuc == 'cong'
        return 1 + 2
    elseif phuongthuc == 'tru'
        return 2 - 1
    elseif phuongthuc == 'nhan'
        return 2*1
    else
        2/1
    end 
  end
  puts result.call('cong')
  ```
- Về gọi lambda thì ta có nhiều cách gọi
```
result.call(10) 
result[10]
result.(10)
```
##sau khi hiểu về block, proc và lambda thì ta đi tìm hiểu sự khác biệt của chúng

###1. Sự khác nhau giữa block và proc

- Proc là objects, còn block thì không
- 1 proc là thể hiện của 1 lớp Proc
```
p = Proc.new { puts "helo helo" }
```
- Điều này cho phép ta gọi phương thức và cũng như gán vào biến và trả lại giá trị của chính proc đó, còn block thì không thể lưu trữ trong một biến và không phải là object
```
p.call  # prints 'helo helo'
p.class # returns 'Proc'
x = p   # x tương đương với p, x Proc instance
p       # trả lại 1 object proc '#<Proc:0x00000002e2a840@(irb):1>'
```
- Block chỉ là một phần trong hàm, nếu nó đứng độc lập thì không có ý nghĩa gì
```
{ puts "helo"}        //SyntaxError: (irb):6: syntax error, unexpected tSTRING_BEG
x = { puts "helo"}   //SyntaxError: (irb):6: syntax error, unexpected tSTRING_BEG
[1,2,3].each {|x| puts x+1}  // pass
```
- Chỉ truyền được 1 block vào trong danh sách đối số của hàm, còn với proc thì có thể truyền n proc vào hàm
```
def call_procs(p1, p2)
  p1.call
  p2.call
end
a = Proc.new { puts "First proc" }
b = Proc.new { puts "Second proc" }
call_procs(a,b)
```

###2. Sự khác nhau giữa proc và lambda

- Về cơ bản thì proc và lambda là giống nhau, mà thực chất Lambda chính là một đối tượng của Proc
```
p = Proc.new { puts "Hello" }      //#<Proc:0x00000002e2cb68@(irb):16> 
lam = lambda { puts "Hello" }      //#<Proc:0x00000002e0b8c8@(irb):17 (lambda)>
p.class      //proc
lam.class   //proc
```
- Sự khác nhau duy nhất giữa lambda và proc là sự trả về đối tượng, và cách gọi
- Lambda kiểm tra số lượng tham số được truyền vào còn Proc thì lại không, nếu không truyền tham số thì proc mặc định tham số đó bằng nil
```
p = Proc.new { |x| puts x +1 }
p.call(1, 2)
# return 2
l = lambda { |x| puts x +1 }
l.call(1, 2)
# return Argument Error
```
- Đồi với hàm dùng return trong lambda và proc thì với proc thì sẽ return ngay sau khi thực hiện xong proc, còn với lambda thì vẫn tiếp tục chạy hết hàm sau khi gọi xong lambda
```
def method_lambda
  lam = lambda { return puts "xin chao" }
  lam.call
  puts "van dung"
end
method_lambda
kết quả in ra là
xin chao
van dung
def method_proc
  proc = Proc.new { return puts "xin chao" }
  proc.call
  puts "van dung"
end
method_proc
kết quả in ra là
xin chao
```
- Vì vậy để xem khi nào dùng proc hay lambda thì ta xem xét việc có dùng return hay không

###3. So sánh lambda và block

- Về sự khác nhau giữa block và lambda giống như giữa block và proc vì labda là một thể hiện của proc

##Sau khi biết về block và lambda, proc. Chúng ta tiếp tục tìm hiểu cách truyền một block, lambda vào function trong Ruby

###1. Truyền block vào function
- Có 2 cách để nhận vào block trong một hàm của Ruby. 
  - Dùng yield:
  ```
def test_yield
  puts yield
end
test_yield { "Nguyen Van Dung" }
  ```
  - Dùng & trước argument
  ```
def test_block(&block)
  puts block.call
end
test_block { "Nguyen Van Dung" }
```
  - Về cách thứ 2 thì nó tạo ra một đối tượng Proc từ bất kể block nào được truyền vào, sau đó đối tượng này thực hiện hàm call
nếu so sánh tốc độ thì cách 2 sẽ lâu hơn bởi vì phải sinh ra đối tượng proc để gọi hàm
  - Các bạn kiểm tra benchmark sẽ thấy
  ```
require "benchmark"
def test_block(&block)
  block.call
end
def test_yield
  yield
end
n = 10000
Benchmark.bmbm do |x|
  x.report("&block") do
    n.times { test_block { "Van Dung" } }
  end
  x.report("yield") do
    n.times { test_yield { "Van Dung" } }
  end
end
Rehearsal ------------------------------------------
&block   0.030000   0.000000   0.030000 (  0.022620)
yield    0.000000   0.000000   0.000000 (  0.004504)
--------------------------------- total: 0.030000sec

             user     system      total        real
&block   0.010000   0.000000   0.010000 (  0.011766)
yield    0.000000   0.000000   0.000000 (  0.003615)
```

###2. Truyền lambda vào function

- Như đã biết chúng ta có thể sử dụng lambda để gán 1 đoạn code dưới dạng 1 variable thì trong code sẽ ngắn gọn và sáng sủa hơn
- yield và & là gì
  - yield: có thể tự động gọi đoạn code mà nó thấy có liên quan
  - &:  đứng trước 1 đối số thì mình có thể nhận biết được đó là 1 lambda, đối số này nên ở vị trí cuối cùng trong 1 dãy đối số truyền vào.
- Ví dụ: ta có 1 lambda sau
 ```
  result =  lambda do |phuongthuc|
    if phuongthuc == 'cong'
        return 1 + 2
    elseif phuongthuc == 'tru'
        return 2 - 1
    elseif phuongthuc == 'nhan'
        return 2*1
    else
        2/1
    end 
  end
  ```
  sau đó define một method tính, trong đó mình truyền vào tham số phuongthuc(phuongthuc này để chọn tính công trừ hay nhân chia trong lambda result)
  ```
  def tinh(phuongthuc)
  yield(phuongthuc)
end
```
Cách để truyền lambda để sử dụng
```
puts tinh('cong', $result)
puts tinh('tru', $result)
```

