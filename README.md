# SignFinder
Tool for easy clean PE32 from AV signature 
 
# Manual [RU]
## Пример работы
Можно посмотреть на [youtube](https://www.youtube.com/watch?v=5I6HMdZfoE8)

## Fast mode
Быстрый режим призван определить какого толка сигнатура нас беспокоит:
* Эмулятора
* Импорта
* Секций

Создаются следующие типы файлов:
* ALL_SECTION - стёрты все секции
* ALL_SECTION_NOT[0].text - стёрты все секции кроме первой, под именем .text
* SECTION[0].text - стёрта только первая секция, под именем .text
* EMUL - на точку входа эмулятора поставлен выход. Он прекратит свою работу и детект эмулятора исчезнет.
* IMPORT - весь импорт перезатёрт - если сигнатура стояла на импорте - она пропадёт.

Формат вызова:

	python SF.py fast path_to_exe 

## Header mode
Методом исключения, если затирание секций оставляет детект - сигнатура стоит на оставшихся заголовках pe32. Данный режим затирает каждое поле по отдельности, создавая файлы типа:

* IMAGE_DOS_HEADER-e_cblp
* IMAGE_OPTIONAL_HEADER-AddressOfEntryPoint

Формат вызова:

	python SF.py head path_to_exe

## Section mode
Это режим работы с конкретной секцией, она делится на 100 участков одинакового размера и в каждом файле стёрт свой участок. Указывается порядковый номер секции.

Формат вызова:

	python SF.py sect path_to_exe section_number

## Manual mode
Режим ручного управления. Заданный в параметрах участок, делится на заданное количество частей, каждая перезатирается по очереди.

Например, было:

	FF FF FF FF FF FF 
Стало:

	00 00 FF FF FF FF 	
	FF FF 00 00 FF FF 	
	FF FF FF FF 00 00
	
Формат вызова:

	python SF.py man path_to_exe offset size part_num

## Manual2 mode
Второй ручной режим, отличается от первого длиной шага, который равен одному байту.

Например, было:

	FF FF FF FF FF FF 
Стало:

	00 00 FF FF FF FF 
	FF 00 00 FF FF FF 
	FF FF 00 00 FF FF 

Формат вызова:

	SF.py man2 path_to_exe offset size window_size

