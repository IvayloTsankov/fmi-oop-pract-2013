# Упражнение 3 — Работа с двоични файлове

## Задача 1

Даден е двоичен файл, съдържащ поне 7 байта. Напишете програма, която разменя байтовете, намиращи се на 7-ма позиция от началото на файла и на 7-ма позиция преди края му.


## Задача 2

Използвайки [структурата за книги](../01/solution/Library.h) от [първото упражнение](../01), създайте двоичен файл, съдържащ последователно записи на 10 книги по ваш избор.

## Задача 3

Даден е двоичен файл `library-bad.bin`, съдържащ последователни записи на книги, в който всички книги, публикувани преди 1999-та година, по погрешка са описани с 20 страници по-малко, отколкото всъщност имат. Напишете програма, създаваща нов двоичен файл, в който тази грешка е коригирана, но който не включва книгите от жанр `CRIMINAL`. За вход може да използвате двоичния файл от предната задача.

# Допълнителни задачи (за домашна работа)

## Задача 4

(Като входни данни за тази задачка може да използвате файловете [mbr.img](mbr.img) и [sdabeginning.img](sdabeginning.img).)

[Master Boot Record](http://en.wikipedia.org/wiki/Master_boot_record) (MBR) е първият сектор на едно storage устройство (като твърд диск и др.). Той съдържа описание на дяловете на устройството и кратка програма, наречена boot loader, която позволява да бъде стартирана определена операционна система. MBR секторът е голям 512 байта и неговият формат е:

<table>
  <thead>
    <tr>
      <th>Описание</th>
      <th>Начален байт</th>
      <th>Брой байтове</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Код на boot-ващата програма</td>
      <td>0</td>
      <td>446</td>
    </tr>
    <tr>
      <td>Описание на първия дял</td>
      <td>446</td>
      <td>16</td>
    </tr>
    <tr>
      <td>Описание на втория дял</td>
      <td>462</td>
      <td>16</td>
    </tr>
    <tr>
      <td>Описание на третия дял</td>
      <td>478</td>
      <td>16</td>
    </tr>
    <tr>
      <td>Описание на четвъртия дял</td>
      <td>494</td>
      <td>16</td>
    </tr>
    <tr>
      <td>Сигнатура — последователността 0x55 0xAA</td>
      <td>510</td>
      <td>2</td>
    </tr>
  </tbody>
</table>

(Забележка: всички числа, започващи с 0x, са числа в шестнадесетичен вид. C++ подържа директно използване на този литерал във вашия код (например: `0x42` е числото 66))

Описанието на всеки дял има следния вид:

*   1 байт — показва дали дялът е активен. Само един от 4-те дяла може да бъде такъв. За да е активен трябва 7-мия бит на този байт да има стойност 1 (ако всички останали битове са със стойност 0, то байта има стойност 0x80);
*   3 байта — не се използват;
*   1 байт — код за тип на дяла. Примерни възможни стойности са:
    -   0x00 — Празен запис за дял (този дял не съществува)
    -   0x05 или 0x0F — Extended дял;
    -   0x83 — Linux дял;
    -   0x82 — Linux swap дял;
    -   0x07 — NTFS дял;
    -   и много други;
*   3 байта — не се използват;
*   4 байта — `unsigned int` число — номер на сектор, от който започва дяла (номерацията започва от 0). Всеки сектор е последователност от 512 байта.
*   4 байта — `unsigned int` — брой сектори, които дялът съдържа

Третирайте еднобайтовите стойности като `unsigned char`.

Сигнатурата е последователността от байтове 0x55 0xAA, която удостоверява, че това е MBR сектор.

Даден е двоичен файл с определена големина (минимум 512 байта). Напишете програма, която проверява дали файлът започва с MBR сектор (проверявайки сигнатурата) и ако да, то извежда пълна информация за дяловете, описани в сектора (извеждете информация само за описанието по-горе типове дялове, а останалите третирайте като непознати). Примерен изход за съответно `mbr.img` и `sdabeginning.img` е:

    Partition 0:
    Active flag set
    Partition type: 0x07 (NTFS partition)
    Starting byte: 32256
    Number of bytes: 104864062464
    Partition 1:
    Partition type: 0x05 (Extended partition)
    Starting byte: 104864125952
    Number of bytes: 107372773888
    Partition 2:
    Partition type: 0x83 (Linux partition)
    Starting byte: 212236899840
    Number of bytes: 107372805120
    Partition 3:
    Partition type: 0x07 (NTFS partition)
    Starting byte: 319609704960
    Number of bytes: 180495544320

    Partition 0:
    Active flag set
    Partition type: 0x83 (Linux partition)
    Starting byte: 1048576
    Number of bytes: 9125756928
    Partition 1:
    Partition type: 0x82 (Linux swap partition)
    Starting byte: 9127853056
    Number of bytes: 1608516608
    Partition 2:
    Empty partition entry
    Partition 3:
    Empty partition entry

За да превърнете номер на сектор/брой сектори в номер на байт/брой байтове умножете съответния номер/брой с литерала `512ull` (литерал за число от тип `unsigned long long`).

Ако сте под Линукс, то бихте могли да иползвате програмата за да прочетете вашия MBR дял, като ѝ подадете пътят до файла на желания дял — обикновено `/dev/sda`.

## Задача 5 — Тайният лабиринт

За тази задача имаме следните enum и структура:

    enum POSITION_OFFSET_TYPE {
        BEGINNING,
        CURRENT,
        END
    };

    struct Node {
        int sign;
        POSITION_OFFSET_TYPE offsetType;
        int left;
        int right;
        unsigned char code[8];
    };

Даден е двоичен файл [labyrinth.bin](labyrinth.bin), съдържащ:

1.  Две int числа;
2.  Масив от 128 `char`-а, който ще наричаме `secret`;
3.  Последователност от обекти от структурата `Node`.

В `secret` е закодирано тайно послание, което вие трябва да разкодирате. Това става с два кода, които се намират в два точно определени възела (`node`-а) от файла. За да ги намерите е необходимо да знаете следното:

*   Възлите са свързани помежду си;
*   Всеки възел съдържа в себе си информация за това кой е следващия възел. Той се определя по следния начин:
    -   Ако `sign` е четно число, то позицията на следващия възел се определя от `offsetType` и `left` полетата;
    -   Ако `sign` е нечетно, то позицията на следващия възел се определя от `offsetType` и `right` полетата;
    -   Ако `offsetType` е `BEGINNING`, то следващия възел е възелът, записан на съответно `left` или `right` байта от началото на файла;
    -   Ако `offsetType` е `CURRENT`, то следващият възел е възелът, записан на `left` или `right` байта спрямо позицията след текущия възел;
    -   Ако `offsetType` е `END`, то следващия възел е възелът, записан на `left` или `right` байта спрямо края на файла (в този случай `left` или сътоветно `right` са отрицателни числа).
*   Търсенето започва от първия възел във файла и продължава последователно в зависимост от това кой възел бъде определен за следващ;
*   Търсените възли са възлите с пореден номер първите две `int` числа, прочетени от файла (първият възел има номер 0). Техните `code` полета съдържат ключа за разкодиране на тайното послание.


За да разкодирате посланието с намерените кодове, използвайте следната функция по веднъж за всеки код:

    void decode(char* secret, unsigned char* code) {
        for (int i = 0; i < 16; i++) {
            for (int j = 0; j < 8; j++) {
                secret[i * 8 + j] ^= code[j];
            }
        }
    }