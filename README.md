# ABAP'da SOLID Prensipleri

### SOLID, yazılım mimarisi için kullanılabilir tasarım kalıplarını tanımlayan bir kısaltmadır. Her harf, bir tasarım ilkesini temsil eder:

  
**S - Single Responsibility Principle (Tek Sorumluluk ilkesi)** : Bir nesnenin sadece bir nedenle değişebilmesi gerektiğini ifade eder. Bu, nesnenin sadece bir görev yerine getirmesi gerektiği anlamına gelir.

  

**O - Open-Closed Principle (Açık-Kapalı ilkesi)** : Bir nesnenin değiştirilmemesi gerekirken, genişletilebileceğini ifade eder. Bu, nesnenin içeriğinin değiştirilmemesi gerekirken, ek fonksiyonalitelerin eklenmesine olanak tanıdığı anlamına gelir.

  

**L - Liskov Substitution Principle (Liskov Substitution ilkesi)** : Alt sınıfların, yukarı sınıfların yerine kullanılabileceğini ifade eder. Bu, alt sınıfların yukarı sınıfların özelliklerini kullanmasına ve yerine kullanmasına olanak tanıdığı anlamına gelir.

  

**I - Interface Segregation Principle (Arayüz Ayrımı ilkesi)** : Kullanıcının ihtiyacı olan ve kullanmayacağı arayüzleri kullanmamasını ifade eder. Bu, arayüzleri kullanıcının ihtiyacı olan ve kullanmayacağı arayüzleri içermemesine olanak tanıdığı anlamına gelir.

  

**D - Dependency Inversion Principle (Bağımlılık Tersine Çevirme ilkesi)** : Yüksek seviyeli modüllerin, düşük seviyeli modülleri kullanmasını ifade eder. Bu, yüksek seviyeli modüllerin düşük seviyeli modülleri kullanmasına ve düşük seviyeli modüllerin yüksek seviyeli modülleri kullanmasına olanak tanıdığı anlamına gelir.

  

**ABAP kodlarında SOLID ilkelerinin uygulanması, daha temiz ve anlaşılır kod yazmanıza ve daha iyi test edilebilir kodlar yazmanıza olanak tanır. Örneğin, SOLID ilkeleri kullanarak, ABAP kodlarınızı daha kolay genişletin**

  

## Single responsibility prenciple


**Single responsibility prenciple kullanarak bir ürünün fiyatını hesaplayan sınıf**

```abap
class lcl_product definition.
	public section.
		data :price type int4,
			  tax_rate type int4.
		methods:
				calculate_price.
endclass.

class lcl_product implementation.
	method calculate_price.
		data: lv_price type i.
		lv_price = me->price + ( me->price \* me->tax_rate / 100 ).
		write lv_price.
	endmethod.
endclass.

```

  

# Open - Closed Prenciple

  

**Open-Closed Principle (OCP) ilkesi, bir sınıfın mevcut davranışlarını değiştirmek yerine, yeni davranışlar eklemek için tasarlanması gerektiği anlamına gelir. OCP'yi uygulamak için, sınıfların genişletilebilir olmasını sağlamak için arayüzler veya soyut sınıflar kullanabilirsiniz.**

  
**Aşağıdaki örnekte, bir "Shape" sınıfı ve onun alt sınıfları olan "Circle" ve "Rectangle" sınıfları gösterilmektedir. "Shape" sınıfı, "draw()" metodunu içermektedir ve bu metod, alt sınıflar tarafından uygulanması gereken bir soyut metoddur. Bu şekilde, "Shape" sınıfı değiştirilmeden yeni şekiller eklenebilir.**

 
```abap
*- Shape class (Parent)
class shape definition abstract.
	public section.
		methods draw abstract.
endclass.

*- circle class
class circle definition inheriting from shape.
	public section.
		methods draw redefinition.
endclass.

*- Circle class implementation
class circle implementation.
	method draw.
		write 'Circle is drawn'.
	endmethod.
endclass.

*- Rectangle class
class rectangle definition inheriting from shape.
	public section.
		methods draw redefinition.
endclass.

*- Rectangle class implementation
class rectangle implementation.
	method draw.
		write 'Rectangle is drawn'.
	endmethod.
endclass.

start-of-selection.

data(lc_circle) = new circle( ).
lc_circle->draw( ).

data(lc_rectangel) = new rectangle( ).
lc_rectangel->draw( ).

```

  

# Liskov Subtiution Prenciple

  

**Liskov Substitution Principle (LSP), bir sınıfın türetilmiş bir sınıfın yerine kullanılmasına izin verirken, programın çalışmasını etkilememesi gerektiğini ifade eder. Örneğin, bir sınıfın türetilmiş bir sınıfı, yerine kullanılabilirse, bu türetilmiş sınıfın kullanılması programın çalışmasını etkilemeyecektir.**

**ABAP kodunda LSP'yi uygulamak için, bir sınıfın türetilmiş bir sınıfının yerine kullanılmasına izin verirken, türetilmiş sınıfın aynı metodları veya özellikleri kullanması gerekir.**

  

```abap
*- lsp_base class
class lsp_base definition.
	public section.
		methods: display.
endclass.

*- lsp_child class
class lsp_child definition inheriting from lsp_base.
	public section.
		methods: display redefinition.
endclass.

*- lsp_base implementation
class lsp_base implementation.
	method display.
		write 'This is the base class'.
	endmethod.
endclass.

*- lsp_child implementation
class lsp_child implementation.
	method display.
		write 'This is the child class'.
	endmethod.
endclass.

start-of-selection.

DATA(base) = NEW lsp_base( ).
base->display( ).

DATA(child) = NEW lsp_child( ).
child->display( ).

DATA(sub) = NEW lsp_child( ).
sub->display( ).
```
 
 

**Bu örnekte, lsp_base sınıfı lsp_child sınıfı tarafından türetilmiştir ve display metodu override edilmiştir. Bu sayede lsp_child sınıfını lsp_base sınıfının yerine kullanabiliriz. Program çalıştırıldığında, lsp_base ve lsp_child sınıflarının display metodları çalıştırılır ve iki farklı sonuç elde edilir.**

  

# Interface Segregation Principle

  

**Interface Segregation Principle (ISP), bir sınıfın kullanmayacağı metodları içermemesi gerektiğini ifade eder. Bu, bir sınıfın sadece kendisi için gerekli olan metodları içeren bir arayüzle sınırlı kalmasını sağlar. Bu sayede, sınıfların sadece kendileri için gerekli olan metodları implemente etmeleri ve diğer metodları kullanmamaları sağlanır.**

**ABAP kodunda ISP'yi uygulamak için, bir sınıfın sadece kendisi için gerekli olan metodları içeren bir arayüzle tanımlanması gerekir.**

  

```abap

interface isp_interface1.
	methods: method1.
endinterface.

interface isp_interface2.
	methods: method2.
endinterface.

  
class isp_class definition.
	public section.
		interfaces isp_interface1.
endclass.


class isp_class implementation.
	method isp_interface1~method1.
		write 'method1 is called implemented from isp_interface1'.
	endmethod.
endclass.

start-of-selection.  

DATA(obj) = NEW isp_class( ).
obj->method1( ).

```

  

**Bu örnekte, isp_class sınıfı sadece isp_interface1 arayüzünü implemente etmektedir. Bu sayede sınıf sadece method1 metodunu kullanmaktadır ve method2 metodunu kullanmamaktadır. Bu sayede sınıf sadece kendisi için gerekli olan metodları implemente etmektedir ve diğer metodları kullanmamaktadır.**

  

# Dependency Inversion Prenciple

  

**Dependency Inversion Principle (DIP), bir sınıfın bağımlı olduğu sınıfların arayüzleri yerine nesnelerini kullanmamasını ifade eder. Bu sayede sınıflar arasındaki bağımlılıklar daha esnek hale gelir ve sınıflar arasındaki değişimlerin etkileri azaltılır.**
  
**ABAP kodunda DIP'yi uygulamak için, bir sınıfın bağımlı olduğu sınıfların arayüzleri yerine nesnelerini kullanması gerekir.**

  

```abap
interface dip_interface.
	methods: method1.
endinterface.

  
class dip_class1 definition.
	public section.
		interfaces dip_interface.
endclass.

class dip_class2 definition.
	public section.
		methods: constructor.
		methods: method2.
	protected section.
		data: di_obj type ref to dip_interface.
endclass.


class dip_class1 implementation.
	method dip_interface~method1.
		write 'method1 is called'.
	endmethod.
endclass.

class dip_class2 implementation.
	method constructor.
		di_obj = new dip_class1( ).
	endmethod.
	
	method method2.
		di_obj->method1( ).
	endmethod.
endclass.

  
DATA(obj2) = NEW dip_class2( ).
obj2->method2( ).

```  

**Bu örnekte, dip_class2 sınıfı dip_class1 sınıfının arayüzü yerine nesnesini kullanmaktadır. Bu sayede dip_class2 sınıfı dip_class1 sınıfının değişimlerinden etkilenmemektedir. Ayrıca dip_class2 sınıfının dip_class1 sınıfının arayüzü yerine nesnesini kullanması sayesinde sınıflar arasındaki bağımlılıklar daha esnek hale gelmektedir.**
