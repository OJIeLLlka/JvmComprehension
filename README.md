JvmComprehension
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
### С запуском программы происходит Class Loading. Запрос делигируется до bootstrap и затем начинается скачивание(если есть что качать)
### Application > Platform > Bootstrap(скачивание) >> Platfrom(скачивание) >> Application(скачивание)
Затем идёт связывание классов Проверка что код валиден, подготовка примитивов в статик полях, связывание ссылок на другие классы)

Классы загружаются в отдел памяти под названием META space
Далее - инициализация и попорядку:

### В разделе STACK MEMORY создается frame с main методом класса JvmComprehens
### В этом фрейме создается примитивная переменная int i = 1; // 1
### В heap(куча) разделе памяти создаётся новый экземпляр класса обжект, А в фрейме main создается ссылка на объект класса Object с именем "o" // 2
### В heap(куча) cоздается новый экземпляр класса Integer А в фрейме main создаётся ссылка на объект класса Integer с именем ii (значение лежит в heap'e же?) // 3
### за счет вызова метода printAll в Stack создаётся новый фрейм, а в нём две СВОИ(новые) ссылочки на параметры :Object o, Integer ii и своя примитивная переменная int i. Ссылкам на объекты классов Integer и Object присваиваются значения(за счет передачи данных в параметры метода при вызове) уже созданных экземпляров ранее, которые хранятся в хипе(тобишь на каждый из этих экземпляров теперь ссылается по две ссылки в программе). Примитивной же int i, что в фрейме метода printAll, присваивается значение примитивной такого же типа и названия(но другой), созданной в методе main(за счет передачи оной в параметры метода printAll) // 4
### В куче создаётся экземпляр класса Integer, а в stack'e, в фрейме метода printALL на него ссылочка uselessVar = 700 // 5
### В куче создаётся новый фрейм метода println. Потом создается еще один фрейм с методом ту стринг, и из него передаются данные с ссылки Object o в фрейм метода println? А потом в фрейме метода println передаётся значение переменной int i созданной в фрейме метода main и ссылка на объект класса Integer ii(именно та, не другая-новая!?), которая тоже была создана в фрейме main. // 6
## Затем после печати в консоли(выполнения метода println) фрейм println удаляется > фрейм printALL удаляется > фрейм main Удаляется. GGWP
