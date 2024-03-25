# Bitrix

## Инфоблоки

Модуль, позволяющий каталогизировать и управлять различными типами (блоками) однородной информации. С помощью информационных блоков может быть реализована публикация различных типов динамической информации: каталоги товаров, блоки новостей, справочники и т.д.

**CIBlockElement** - класс для работы с элементами информационных блоков.

_GetProperty_ - возвращает значения свойства для элемента element_id. Метод статический.

[Документация](https://dev.1c-bitrix.ru/api_help/iblock/classes/ciblockelement/getproperty.php)

_GetList_ - Возвращает список элементов по фильтру arFilter. Метод статический.

[Документация](https://dev.1c-bitrix.ru/api_help/iblock/classes/ciblockelement/getlist.php)

Чтобы получить необходимый инфоблок, необходимо в качестве параметра в index.php компонента передать ID инфоблока.

Пример:

`$APPLICATION->IncludeComponent(
  'egor:mycomponent',
  '.default',
  [
    'IBLOCK_ID' => '11',
    'IBLOCK_TYPE' => 'catalog',
    'COMPONENT_TEMPLATE' => '.default',
  ],
  false
);`

Посмотреть свойства элементов можно в админке в карточке сущности инфоблока, используя DEV*TOOLS, либо в разделе *Инфоблоки->{РАЗДЕЛ ИНФОБЛОКА}->Свойства\*

## Получение AJAX

Чтобы получить и обработать запрос AJAX в Bitrix необходимо использовать следующее API:

`$request = \Bitrix\Main\Context::getCurrent()->getRequest();
$value = $request->getPost("path");
`
Где _$value_ - это параметр POST запроса.

Тело запроса отправляется в формате - _application/x-www-form-urlencoded_

[Документация](https://dev.1c-bitrix.ru/api_d7/bitrix/main/request/index.php)

## Проверить наличие файла

Чтобы проверить наличие файла, можно воспольтзоваться специальным классом BITRIX - `string
CFile::CheckFile(
	array file,
	int max_size = 0,
	string mime_types = false,
	string Ext = false,
	bool ForceMD5=false,
	bool SkipExt=false
)`
[Документация](https://dev.1c-bitrix.ru/api_help/main/reference/cfile/checkfile.php)
