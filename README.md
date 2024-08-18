Шпаргалка Go

Большинство примеров кода взяты из экскурсии по Go, которая является отличным введением в Go. Если вы новичок в Go, проведите эту экскурсию. Серьезно.

Базовый синтаксис

Привет, мир

Файл hello.go:
пакетосновной 

импортируйте "fmt"

функцию main() { 
 fmt.Println("Привет, Go") 
}

$ go run hello.go
Операторы

Арифметика

Оператор
Описание
+
дополнение
-
вычитание
*
умножение
/
коэффициент
%
остальное
&
побитовый и
|
побитовый или
^
побитовый xor
&^
немного понятно (и не очень)
<<
сдвиг влево
>>
сдвиг вправо
Сравнение

Оператор
Описание
==
равный
!=
не равны
<
меньше, чем
<=
меньше или равно
>
больше, чем
>=
больше или равно
Логический

Оператор
Описание
&&
логические и
||
логический или
!
логический не
Другое

Оператор
Описание
&
адрес указателя / create
*
указатель разыменования
<-
оператор отправки / получения (см. "Каналы" ниже)
Объявления

Тип следует за идентификатором!
foo var // объявление с инициализацией 42
= int foo var // объявление без инициализации int
foo var, bar int = 42, 1302 // объявляйте и инициализируйте сразу несколько переменных
var foo = 42 // тип опущен, будет выведен
foo := 42 // сокращение , только в телах функций, опустите ключевое слово var, тип всегда неявный
const константа = "Это константа"

// iota может использоваться для увеличения чисел, начиная с 0
const (
 _ = йота
    a
    b
    c = 1 << йота
    d 
)
 fmt.Println(a, b) // 1 2 (0 пропущено)
    fmt.Println(c, d) // 8 16 (2^3, 2^4) 

Функции

functionName
func // простая функция() {}

// функция с параметрами (опять же, типы идут после идентификаторов)
func functionName(param1 string, param2 int) {}

// несколько параметров одного типа
func functionName(param1, param2 int) {}

// объявление возвращаемого типа
func functionName() int {
 Return 42
}

// Может возвращать несколько значений одновременно
func returnMulti() (int, string) {
 return
 42, "foobar"
}
var x, str = returnMulti()

// Возвращает результаты с несколькими именами, просто возвращая
func returnMulti2() (n int, s string) {
 n = 42
    s = "foobar"
    // n и s будут возвращены
    return
}
var x, str = returnMulti2()

Функции как значения и замыкания

func main() {
    // Назначить функцию имени
    add := func(a, b int) int {
        return a + b
    }
// Используйте имя для вызова функции
    fmt.Println(add(3, 4))
}

// Замыкания, лексически охваты: Функции могут получить доступ к значениям, которые были 
   в области видимости при определении функции
func scope() func() int{
    outer_var := 2
    foo := func() int { return outer_var}
    return foo
}

func another_scope() func() int{
// Не будет компилироваться, потому что outer_var и foo не определены в этой области    outer_var = 444
    return foo
}


// Закрытие
func outer() (func() int, int) {
    outer_var := 2
    inner := func() int {
        outer_var += 99 // Outer_var из внешней области мутирован.
        return outer_var
    }
    inner()
    return inner, outer_var // Вернуть внутреннюю функцию и мутировав outer_var 101
}

Вариативные функции

main func() { 
 fmt.Println(сумматор(1, 2, 3)) // 6
	fmt.Println(сумматор(9, 9)) // 18

	чисел := []int{10, 20, 30} 
 fmt.Println(сумматор(числа...)) // 60
}

// С помощью ... перед именем типа последнего параметра вы можете указать, что он принимает ноль или более из этих параметров.
// Функция вызывается как любая другая функция, за исключением того, что мы можем передавать столько аргументов, сколько захотим.
func adder(args ...int) int{ 
 total := 0
for _, v := range args { // Перебирает аргументы независимо от их числа.
		total += v
 }
 return total 
}

Встроенные типы

bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // псевдоним для uint8

rune // псевдоним для int32 ~= символ (кодовая точка Unicode) - очень простой

float32 float64

complex64 complex128.                                                                                
Все предварительно объявленные идентификаторы Go определены во встроенном пакете.
Преобразования типов

float64 = float64 f var
42 = int i var(i) 
var u uint = uint(f)

// альтернативный синтаксис
i := 42
f := float64(i)
u := uint(f)

Упаковка

	•	Объявление пакета в верхней части каждого исходного файла
	•	Исполняемые файлы находятся в пакете main
	•	Соглашение: имя пакета == последнее имя пути импорта (путь импорта math/rand => пакет rand)
	•	Идентификатор в верхнем регистре: экспортирован (виден из других пакетов)
	•	Идентификатор в нижнем регистре: private (не виден в других пакетах)
Структуры управления

Если

func main() {
 // Базовый , 
	if x > 10 {
		return x
	} else if x == 10 {
		return 10
	} else {
		return -x
	}
 // Перед условием можно поставить одно утверждение, 
    if a := b + c; a < 42 {
		return a
	} else {
		return a - 42
	}

 // Введите утверждение внутри 
	  {} = "foo"
	if str, ok := val.(строка); ok{ 
 fmt.Println(str)
 }
}

Циклы

    // Есть только `for`, не `while`, не `until`
    for i := 1; i < 10; i++ {
    }
    for ; i < 10;  { // while - loop
    }
    for i < 10  { // Вы можете опустить точку с запятой, если есть только условие
    }
    for { //Вы можете опустить условие ~ while (true)
    }
    
    // Используйте break/continue в текущем цикле 
    // используйте break/continue с меткой на внешнем цикле
Здесь:
    for i := 0; i < 2; i++ {
        for j := i + 1; j < 3; j++ {
            if i == 0 {
                continue here
            }
            fmt.Println(j)
            if j == 2 {
                break
            }
        }
    }

Там:    for i := 0; i < 2; i++ {
        for j := i + 1; j < 3; j++ {
            if j == 1 {
                continue
            }
            fmt.Println(j)
            if j == 2 {
                break there
            }
        }
    }

Переключение Switch

    // Заявление о переключении
    switch operatingSystem {
    case "darwin":
        fmt.Println("Mac OS Hipster")
        // Случаи ломаются автоматически, без прорыва default
    case "linux":
        fmt.Println("Linux Geek")
    default:
        // Windows, BSD, ...
        fmt.Println("Other")
    }

    // Как с for и if, вы можете иметь оператор назначения перед значением переключения
    switch os := runtime.GOOS; os {
    case "darwin": ...
    }

// Вы также можете проводить сравнения в случаях переключения    number := 42
    switch {
        case number < 42:
            fmt.Println("Smaller")
        case number == 42:
            fmt.Println("Equal")
        case number > 42:
            fmt.Println("Greater")
    }

// Случаи могут быть представлены в списках, разделенных запятыми    
        var char byte = '?'
        switch char {
        case ' ', '?', '&', '=', '#', '+', '%':
            fmt.Println("Should escape")
    }

Массивы, срезы, диапазоны

Массивы

a var [10]int // объявляет массив int длиной 10. Длина массива является частью типа!
a[3] = 42     // Установить элементы
i := a[3] 


  = [2]int{1, 2,,,}
a := [2]int{1, 2} // сокращение
a := [...]int{1, 2} // elipsis -> Компилятор вычисляет длину массива

Срезы

var a []int                              // Объявить фрагмент - похож на массив, но длина не указана
var a = []int {1, 2, 3, 4}               // Объявить и инициализировать фрагмент (подтерженый массивом, заданным неявно)
a := []int{1, 2, 3, 4}                   // короткое объявление 
chars := []string{0:"a", 2:"c", 1: "b"}  // ["a", "b", "c"]

var b = a[lo:hi]	// Создает срез (вид массива) из index lo to hi-1
var b = a[1:4]		// срез  изindex 1 to 3
var b = a[:3]		// Отсутствует низкий Индекс implies 0
var b = a[3:]		// Отсутствует высокий индекс implies len(a)
a =  append(a,17,3)	// добавление элемента к срезу a
c := append(a,b...)	// объединить срезы  a and b

//создаем фрагмент с помощью make
a = make([]byte, 5, 5)	// первая длина второе вместимость
a = make([]byte, 5)	// емкость не является обязательной

// создаём срез массива
x := [3]string{"Лайка", "Белка", "Стрелка"}
s := x[:] // срез, ссылающийся на хранилище x

Операции с массивами и фрагментами

len(a) указывает длину массива / фрагмента. Это встроенная функция, а не атрибут / метод в массиве.
// перебираю массив / фрагмент,
for i, e := range a {
 // i - индекс, e - элемент
}

// если вам нужно только e:
for _, e := range a {
 // e - это элемент
}

// ... и если вам нужен только индекс
for i := range a {
}

// В Go до версии 1.4 вы получите ошибку компилятора, если не используете i и e.
// В Go 1.4 введена форма без переменных, так что вы можете делать это
for range time.Tick(time.Second) {
 // делайте это один раз в секунду 
}

Карты

make := m(map[string]int)
m["m["key"] = 42
"] = 42
fmt.Println(m["m["key"] = 42
"])

удалить(m, "m["key"] = 42
")

elem, ok := m["m["key"] = 42
"] // проверьте, присутствует ли ключ "key", и извлеките его, если да

// литерал карты
var m = map[string]Vertex{
 "Bell Labs": {40.68433, -74.39967}, 
 "Google": {37.42202, -122.08408}, 
}

// перебираем содержимое карты
for key, value := range m {
}

Структуры

Классов нет, только структуры. Структуры могут иметь методы.
// Структура - это тип. Это также коллекция полей
// объявление
type Vertex struct {
    X, Y float64
}

// Создание
var v = Vertex{1, 2}
var v = Vertex{X: 1, Y: 2} // Создает структуру, определяя значения с помощью ключей
var v = []Vertex{{1,2},{5,2},{5,5}} // Инициализировать фрагмент структур

// Доступ к Полям структуры
v.X = 4

// Вы можете объявить методы на структурах. Структура, на которой вы хотите объявить 
// метод (получающий тип), находится между ключевым словом func и 
// именем метода. Структура копируется при каждом вызове метода(!)
func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

// вызов Метода
v.Abs()

// Для методов мутации вам нужно использовать указатель (см. ниже) на структуру
// Как тип. При этом значение структуры не копируется для вызова метода.
func (v *Vertex) add(n float64) {
    v.X += n
    v.Y += n
}

Анонимные структуры: дешевле и безопаснее, чем использование map[string]interface{}.
struct := point {
 X, Y int
}{1, 2}

Указатели

Vertex := p{1, 2} // p - вершина
q := &p            // q - указатель на вершину
r := &Vertex{1, 2} // r также является указателем на вершину

// Тип указателя на вершину - *Vertex

 var s *Vertex = new(Вершина) // new создает указатель на новый экземпляр структуры

Интерфейсы

интерфейс
Awesomizer тип // объявление интерфейса {
 Awesomize() string
}

// типы * не * объявляются для реализации интерфейсов
типа Foo struct {}

// вместо этого типы неявно удовлетворяют интерфейсу, если они реализуют все требуемые методы
func (foo Foo) Awesomize() string {
 возвращает "Потрясающе!" 
}

Встраивание

В Go нет подклассов. Вместо этого есть встраивание интерфейса и структуры.
интерфейс
ReadWriter тип // Реализации ReadWriter должны удовлетворять требованиям как Reader, так и Writer {
 Reader
    Writer
}

// Server предоставляет все методы, которые есть у Logger, 
типа Server struct {
 Host string
    Port int
    *log.Logger
}

// инициализируйте встроенный тип обычным способом
server := &Server{"localhost", 80, log.New(...)}

// методы, реализованные во встроенной структуре, передаются через 
сервер.Log(...) // вызывает сервер.Logger.Log(...)

// имя поля встроенного типа - это имя его типа (в данном случае Logger)
var logger *log.Logger = сервер.Logger

Ошибки

Здесь нет обработки исключений. Вместо этого функции, которые могут выдавать ошибку, просто объявляют дополнительное возвращаемое значение типа error. Это error интерфейс:
тип
ошибки
интерфейса // с нулевым значением, не представляющим ошибки. // Тип встроенного интерфейса error - это обычный интерфейс для представления состояния ошибки, {
 Ошибка() строка 
}

Вот пример:
sqrt функция(x float64) (float64, ошибка) {
 если x < 0{ 
 возвращает 0, ошибки.Новое("отрицательное значение")
 }
 возвращает математику.Sqrt(x), ноль
}

функция main() { 
 val, err := sqrt(-1)
 если ошибка != ноль {
 // обрабатывать ошибку
		fmt.Println(ошибка) // 
		возвращает отрицательное значение
 }
 // Все в порядке, используйте `val`.
	fmt.Println(val) 
}

Параллелизм

Goroutines

Goroutines - это облегченные потоки (управляемые Go, а не потоками операционной системы). go f(a, b) запускает новую goroutine, которая выполняется f (задается f функция).
doStuff
func // просто функция (которую позже можно запустить как goroutine)(s string) {
}

функция main() {
 // использование именованной функции в goroutine
    go doStuff("foobar")

 // использование анонимной внутренней функции в goroutine
    go func (x int) {
 // тело функции находится здесь
 }(42)
}

Каналы

ch := make(chan int) //// создаем канал типа int
ch <- 42             // Отправляем значение в канал ch.
v := <-ch            // Получаем значение из ch
/// Блок небуферизованных каналов. Чтение блоков, когда значение недоступно, запись блоков до тех пор, пока не произойдет чтение.

// Создаем буферизованный канал. Запись в буферизованные каналы не блокируется, если записано менее <размера буфера> непрочитанных значений.
ch := make(chan int, 100)

close(ch) /// закрывает канал (закрывать должен только отправитель)

// читаем из канала и проверяем, был ли он закрыт

v, ok := <-ch

// если ok имеет значение false, канал закрыт

// Читаем из канала, пока он не закроется
for i := range ch {
    fmt.Println(i)
}

// выбор блоков при операциях с несколькими каналами, если один из них разблокируется, выполняется соответствующий случай
func doStuff(channelOut, channelIn chan int) {
    select {
    case channelOut <- 42:
        fmt.Println("We could write to channelOut!")
    case x := <- channelIn:
        fmt.Println("We could read from channelIn")
    case <-time.After(time.Second * 1):
        fmt.Println("timeout")
    }
}

Аксиомы канала

	•	Отправка на нулевой канал блокируется навсегда var c изменяет строку
	•	c <- "Привет, мир!"
	•	// фатальная ошибка: все программы находятся в спящем режиме - взаимоблокировка!  
	•	Прием по нулевому каналу блокируется навсегда fmt string chan c
	•	var.Println(<-c) 
	•	// неустранимая ошибка: все программы находятся в спящем режиме - взаимоблокировка!  
	•	Отправка на закрытый канал вызывает панику make = c var(chan string, 1) 
	•	c <- "Привет, мир!"
	•	закрыть(c) 
	•	c <- "Привет, паника!"
	•	// паника: отправка по закрытому каналу  
	•	Прием по закрытому каналу немедленно возвращает нулевое значение make = c var(chan int, 2) 
	•	c <- 1
	•	c <- 2
	•	закрыть(c) 
	•	для i := 0; i < 3; i++{ 
	•	 fmt.Printf("%d ", <-c)
	•	}
	•	// 1 2 0  
Печать

fmt.Println("Привет, 你好, p
" // базовая печать плюс перевод строки struct { X, Y int }{ 17, 2} 
fmt.Println( "Моя точка зрения:", p, "x coord=", p.X ) // печатать структуры, целые числа и т.д.
s := fmt.Sprintln( "Моя точка зрения:", p, "x coord=", p.X ) / / вывести в строковую переменную

fmt.Printf("%d hex:%x bin:%b fp:%f sci:%e",17,17,17,17.0,17.0) // формат c-ish
s2 := fmt.Sprintf( "%d %f", 17, 17.0 ) // форматировать печать в строку переменная

hellomsg := `
 "Hello" по-китайски - это 你好 ("Ни Хао")
 "Hello" по-хинди - это 
' // многострочный строковый литерал с обратным тиком в начале и конце

Reflection

Type Switch
Переключение типов похоже на обычную инструкцию switch, но регистры в переключателе типов определяют типы (не значения), которые сравниваются с типом значения, удерживаемого данным значением интерфейса.
do func(i интерфейс{}) {
 переключить v := i.(тип) { 
 case int:
 fmt.Printf("Дважды %v равно %v\n", v, v*2)
 строка с регистром :
 fmt.Printf("%q имеет длину в %v байт\n", v, len(v))
 По умолчанию:
 fmt.Printf("Я не знаю о типе %T!\n", v)
 }
}

функция main() { 
 делать(21)
 do("привет")
 do(true) 
}

Snippets


Встраивание файлов

Программы Go могут встраивать статические файлы, используя "embed" пакет следующим образом:
пакетосновной 

импорт (
 "встроить"
	"войти"
	"net/ http" 
)

// content содержит статическое содержимое (2 файла) для веб-сервера.
//go:embed a.txt b.txt 
вар содержимого embed.FS

функция main() { 
 http.Обработайте("/", http.Файловый сервер(http.FS(содержимое)))
 журнал.Фатальный(http.ListenAndServe(":8080", ноль)) 
}

Полный пример игровой площадки
HTTP - сервер

package main

import (
    "fmt"
    "net/http"
)

// Определить тип ответа
type Hello struct{}

// Пусть этот тип реализует метод ServeHTTP (определенный в интерфейсе http.Handler)
func (h Hello) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    fmt.Fprint(w, "Hello!")
}

func main() {
    var h Hello
    http.ListenAndServe("localhost:4000", h)
}

// Here's the method signature of http.ServeHTTP:
// type Handler interface {
//     ServeHTTP(w http.ResponseWriter, r *http.Request)
// }
