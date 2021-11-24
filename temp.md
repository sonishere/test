# Bài báo cáo tìm hiểu về các mẫu thiết kế
## Môn lập trình hướng đối tượng
### Lớp INT_2204_3

#### Danh sách thành viên:
* Nguyễn Trường Sơn - MSV: 19020609
* Nguyễn Hoàng Hiệp - MSV: 
* Phạm Thanh Ngân - MSV: 

#### **Về việc làm bài báo cáo**: 
- Mỗi thành viên sẽ làm phần việc mình được giao và sẽ commit vào repo chung của nhóm khi đã hoàn tất (**[link repo của nhóm](https://github.com/sonishere/oop-2021)**)
- **[Link repo làm tài liệu tham khảo và phân tích](https://github.com/youlookwhat/DesignPattern)**

## Nội dung báo cáo:
### **A. Creational Pattern**
#### ***1. Abstract Factory***
- Giống nhau: 
  -  Pattern có cấu tạo như sau:
  1. AbstractFactory: interface *XianRoujiaMoYLFoctory* chứa công đoạn làm món ăn **Rou Jia Mo** (Bánh mì kẹp Tây An)
  2. Product: Các đối tượng Meet, Yuanlao
  3. ConcreteFactory: Các subclass của các Product như: ChangShaFreshMeet, ChangShaTeSeYuanLiao, XianTeSeYuanLiao...
  4. Client: **Rou Jia Mo**
- Khác nhau: 
  - Không tồn tại AbstractProduct, các Method (Meet, YuanLao) được khởi tạo chỉ dưới dạng class thường, không phải abstract hoặc interface
```
                  package com.example.jingbin.designpattern.factory.cxgc;

                  /**
                   * Created by jingbin on 2016/10/26.
                   */

                  public class Meet {
                  }
```

#### ***2. Factory Method***
- Giống nhau:
  - Cấu tạo của Pattern:
  1. Super Class: Class RoujiaMoStore đại diện cho các cửa hàng bán **Rou Jia Mo**, chứa các công đoạn chuẩn bị và xuất kho
  2. Sub Class: Các chi nhánh: XianKuRoujiMo, XianSuanRoujiMo, XianlaRoujiMo, XianSuanRoujiMo
  3. Factory: Class XianSimpleRoujiaMoFactory xử lí input và đưa về các chi nhánh (sub class)
- Khác nhau:
  - Không có sự khác nhau, Pattern đang xét tuân thủ đúng theo cấu trúc mẫu
 
 #### ***3. Builder***
 - Giống nhau:
 
<p align="center">
 <img src="https://github.com/sonishere/sonishere/blob/main/UML.png" width="500">
</p>

 - Như ta có thể thấy theo hình trên, pattern builder của repo có cấu trúc tương tự như của Gang Of Four (GOF):
    - Builder: (Builder.java) là 1 abstract class và được kế thừa bởi 1 ConcreteBuilder (ConcreteBuider.java)
    - Director: (Director.java) để gọi tới Builder
    - Product: đại diện cho đối tượng cần tạo, đối tượng này phức tạp, có nhiều thuộc tính
- Khác nhau: Không có sự khác nhau, Pattern đang xét tuân thủ đúng theo cấu trúc mẫu

 #### ***4. Prototype***
 - Giống nhau:
    - Prototype: Class Shape đóng vai trò khởi tạo một hình nói chung
    - ConcretePrototype: Các class con đóng vai trò clone theo yêu cầu (Circle, Rectangle, Square)
    - Client: Class ShapeCache đóng vai trò yêu cầu Shape clone các hình (Circle, Rectangle, Square)
```
              public class ShapeCache {

              private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();

              public static Shape getShape(String shapeId) {
                  Shape shapeCache = shapeMap.get(shapeId);
                  return (Shape) shapeCache.clone();
              }

              public static void loadCache() {
                  Circle circle = new Circle();
                  circle.setId("1");
                  shapeMap.put(circle.getId(), circle);

                  Rectangle rectangle = new Rectangle();
                  rectangle.setId("2");
                  shapeMap.put(rectangle.getId(), rectangle);

                  Square square = new Square();
                  square.setId("3");
                  shapeMap.put(square.getId(), square);
              }
          }
```
- Khác nhau: Không có sự khác nhau, Pattern đang xét tuân thủ đúng theo cấu trúc mẫu

#### ***5. Singleton***
- Giống nhau: Đều tuân thủ các nguyên tắc cơ bản của Implement Singleton Pattern:
    - private constructor để hạn chế truy cập từ class bên ngoài.
    - Đặt private static final variable đảm bảo biến chỉ được khởi tạo trong class.
    - Có một method public static để return instance được khởi tạo ở trên.
- Repo có một số loại Implement khá hiệu quả như: 
  - Double Check Locking Singleton:
```
        private static SingletonLanHan singletonLanHanFour;

        public static SingletonLanHan getSingletonLanHanFour() {
            if (singletonLanHanFour == null) {
                synchronized (SingletonLanHan.class) {
                    if (singletonLanHanFour == null) {
                        singletonLanHanFour = new SingletonLanHan();
                    }
                }
            }
            return singletonLanHanFour;
        }
```

   > Cách này đã khắc phục được nhược điểm của cách Thread Safe Singleton (một phương thức synchronized sẽ chạy rất chậm và tốn hiệu năng, bất kỳ Thread nào gọi đến đều phải chờ nếu có một Thread khác đang sử dụng. Có những tác vụ xử lý trước và sau khi tạo thể hiện không cần thiết phải block). Để implement theo cách này, chúng ta sẽ kiểm tra sự tồn tại thể hiện của lớp, với sự hổ trợ của đồng bộ hóa, hai lần trước khi khởi tạo. Phải khai báo volatile cho instance để tránh lớp làm việc không chính xác do quá trình tối ưu hóa của trình biên dịch.
    
   - Bill Pugh Singleton Implementation:
  
```
      public class SingletonIn {

          private SingletonIn() {
          }

          private static class SingletonInHodler {
              private static SingletonIn singletonIn = new SingletonIn();
          }

          public static SingletonIn getSingletonIn() {
              return SingletonInHodler.singletonIn;
          }
      }
```

  >Với cách làm này bạn sẽ tạo ra static nested class với vai trò 1 Helper khi muốn tách biệt chức năng cho 1 class function rõ ràng hơn. Đây là cách thường hay được sử dụng và có hiệu suất tốt (theo các chuyên gia đánh giá). Khi Singleton được tải vào bộ nhớ thì SingletonInHodler chưa được tải vào. Nó chỉ được tải khi và chỉ khi phương thức getSingletonIn() được gọi. Với cách này tránh được lỗi cơ chế khởi tạo instance của Singleton trong Multi-Thread, performance cao do tách biệt được quá trình xử lý. Do đó, cách làm này được đánh giá là cách triển khai Singleton nhanh và hiệu quả nhất.

- Khác nhau: Không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

### **B. Structural Pattern**
#### ***1. Adapter***
  Adapter Pattern (Người chuyển đổi) là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Adapter Pattern cho phép các inteface (giao diện) không liên quan tới nhau có thể làm việc cùng nhau. Đối tượng giúp kết nối các interface gọi là Adapter.
- Giống nhau: Pattern bao gồm các thành phần cơ bản sau:
  1. Adaptee
  2. Adapter
  3. Target
  4. Client
  - Trong đó giao diện trên một chiếc điện thoại di động tốn 5V, điện áp gia đình là 220, vậy nên ta cần 1 Adapter (cụ thể là class V5PowerAdapter) để chuyển đổi điện áp 220V sang 5V:

```
          public class V5PowerAdapter implements V5Power {

              private int v220power;

              public V5PowerAdapter(V220Power v220Power) {
                  v220power = v220Power.provideV220Power();
              }

              @Override
              public int provideV5Power() {

                  Log.e("---", "适配器: 经过复杂的操作,将" + v220power + "v电压转为5v");
                  return 5;
              }
          }
```

-Khác nhau: Về cơ bản, không có quá nhiều sự khác biệt rõ rệt, cơ bản Pattern tuân thủ theo GOF

#### ***2. Bridge***
- Bridge Pattern là một trong những Pattern thuộc nhóm cấu trúc (Structural Pattern). Ý tưởng của nó là tách tính trừu tượng (abstraction) ra khỏi tính hiện thực (implementation) của nó. Từ đó có thể dễ dàng chỉnh sửa hoặc thay thế mà không làm ảnh hưởng đến những nơi có sử dụng lớp ban đầu.
- Giống nhau:
   -  Cấu trúc Pattern Bridge:
   1. Client: đại diện cho khách hàng sử dụng các chức năng thông qua Abstraction.
   2. Abstraction : định ra một abstract interface quản lý việc tham chiếu đến đối tượng hiện thực cụ thể (Implementor).
   3. Refined Abstraction (AbstractionImpl) : hiện thực (implement) các phương thức đã được định ra trong Abstraction bằng cách sử dụng một tham chiếu đến một đối tượng của      Implementer.
   4. Implementor : định ra các interface cho các lớp hiện thực. Thông thường nó là interface định ra các tác vụ nào đó của Abstraction.
   5. ConcreteImplementor : hiện thực Implementor interface.
   - Trong đó repo đã sử dụng các ví dụ về vẽ vòng tròn và màu sắc gồm các bước:
   1. 
#### ***3. Composite***
#### ***4. Decorator***
#### ***5. Facade***
#### ***6. Flyweight***
#### ***7. Proxy***


### **C. Behavioral Pattern**
#### ***1. Chain of Responsibility***
#### ***2. Command***
#### ***3. Interpreter***
#### ***4. Iterator***
#### ***5. Mediator***
#### ***6. Memento***
#### ***7. Observer***
#### ***8. State***
#### ***9. State***
#### ***10. Strategy***
#### ***11. Template Method***
#### ***12. Visitor***
 
 
 
 
 
