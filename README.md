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

## Создание веб-страницы в bitrix

Чтобы создать страницу в Bitrix, необходимо в корне проекта создать директорию и назвать ее именем страницы.
В директории необходимо создать файл index.php. В нем вызываются хедер и футер:
`<? require($_SERVER['DOCUMENT_ROOT'] . '/bitrix/header.php');`
`<? require($_SERVER['DOCUMENT_ROOT'] . '/bitrix/footer.php');?>`

Между вызовами хедера и футера необзодимо ключить шаблон компонента:

`
$APPLICATION->IncludeComponent(
'toledo:seasonal.dom-i-sad',
'.default',
[
'ELEMENT_ID_SEASONAL' => 196717,
'IBLOCK_ID' => 15,
'IBLOCK_ID_MULTIREGION' => 37,
'BASE_PRICE' => '7',
'CHUNKS' => [8, 7, 8, 9],
'CAROUSEL' => [
[109718, 109717, 107995, 108906, 112141, 112770, 129255, 196360],
],
'DETAIL_LINK' => '/products/',
'SEASON_TYPE' => 'dom-i-sad',
'SERVER_IMAGES' => 'https://img.toledo24.pro/'
],
false
);
`

Шаблон компонента создается в директории `local/components`. 
Вторым параметром указывается папка в которой расположен шаблон компонента. В данном случае это `.default`.
Третим параметром передается массив с параметрами, которые затем можно использовать в шаблоне компонента.

В директории компонента обязательно необходимо создать файл class.php.

Это файл класса который наследуется от глобального класса Bitrix `CBitrixComponent`.
В классе для инициализации компонента необходимо создать публичный метод `executeComponent`.

Вот пример компонента:

`
class SeasonDacha extends CBitrixComponent
{

    private array $basket = [];

    public function executeComponent(): void
    {
        $arBaseBlockInfo = $this->getBaseBlockInfo();
        $allProductIdList = $this->mergeCarouselIdList($arBaseBlockInfo);
        $allProductInfoList = $this->getInfoProductCardInfo($allProductIdList);
        $splitedProductIdList = $this->splitProductIdList($arBaseBlockInfo);
        $this->arResult = [
            'PROMO_INFO' => $arBaseBlockInfo,
            'PRODUCT_INFO_LIST' => $allProductInfoList,
            'CHUNCKS_ID_LIST' => $splitedProductIdList,
            'CAROUSEL_ID_LIST' => $this->arParams['CAROUSEL'],
            'BASKET' => $this->basket,
        ];

        $this->includeComponentTemplate();
    }

    ... 
}
`

Для передачи данных из класса в шаблон используется зарезервированная переменная `$arResult`, которая является ассоциативным массивом.
Для ренедеринга компонета используется метод глоьального класса `includeComponentTemplate()`.

В директории .default создается файл `template.php,` в котором прописывается вертска компонента.
По соседству в этой диреткории можно создать файл `style.css,` который будет автоматические обрабатывать стили компонента.
