# Схема таблицы для сбора данных о находках таксонов из научных публикаций в терминах DarwinCore

В этом документе описывается полный список полей, которые требуются для сбора и публикации литературных данных типа occurence — т.е. о находке таксона в конкретном месте в указанную дату.

Для интероперабельности данных используются [термины стандарта DarwinCore](https://dwc.tdwg.org/terms/).

Для публикации данных через GBIF, к наборам такого типа предъявляются определённые [требования по качеству данных](https://www.gbif.org/data-quality-requirements-occurrences).

Необходимый минимум для данных о находках включает следующие указания:
- уникальный идентификатор находки
- основание находки
- название таксона
- ранг таксона
- царство
- дата
- код страны
- десятичные координаты
- географическая система координат
- точность координат

Извлечение данных из литературы подразумевает, что часть полей будет заполнено вручную точным копированием оригинального текста из источника (поля, начинающиеся с "verbatim"), а часть — заполнена [автоматически] на основе разбора оригиналов. Все требуемые поля для данных такого типа могут быть получены разбором оригинального текста.

Поэтому операторам, кто будет разбирать публикации и вносить данные, предоставляется сокращённая таблица только с нужными полями.

## Сокращённая таблица для операторов, кто будет вносить данные

Перечень полей (разбиты по классам терминов DarwinCore).

**Категория `Record-level`**

- **`bibliographicCitation`** — полная библиографическая ссылка на публикацию (литературный источник находки).

Информация в этом поле является основанием для всей предоставляемой в конкретной записи информации о находке. Является указанием, каким образом следует цитировать конкретную запись при использовании данных.

- **`sourceID`** — порядок записи таксона или находки внутри литературного источника.

Часто бывают случаи, когда виды в публикации располагаются не по алфавиту, а по системе принятой классификации. Это поле используется для сохранения порядка указания таксонов или находок, как они перечислялись в публикации. Возможны два варианта: если имеется сквозная нумерация таксонов, она заносится как есть, даже если для них даются несколько находок; если сквозная нумерация отсутствует, тогда находки вносятся по порядку перечисления, и после внесения всей публикации это поле последовательно заполняется по увеличению.

Не является термином Darwin Core.

**Класс `Identification`**

- **`verbatimIdentification`** — таксономическая идентификация образца или наблюдения точно в том виде, в котором она указана в оригинале в публикации.

В это поле досимвольно копируется название таксона — точно так, как оно представлено в оригинале.

Парсинг оригинального названия затем может проходить по двум вариантам. В первом варианте название ищется в Index Fungorum и человек сам решает, какое больше всего подходит, учитывая все внутривидовые таксоны и авторов базионимов, замещающих синонимов и комбинаций. В этом случае оператор обычно сам заполняет поле `scientificName`, выбирая подходящее из предлагаемого списка названий, взятых из Index Fungorum. Во втором варианте парсинг осуществляется инструментами GBIF ([Species API](https://www.gbif.org/developer/species)), и наиболее подходящие названия выбираются автоматически. В этом случае будут найдены не все названия, и не все из найденных будут корректно интерпретированы. Возможно последовательное использование первого и второго вариантов. Решение принимается исходя из задач проекта.

- **`scientificName`** — полное научное название таксона с указанием авторства, взятое из Index Fungorum или GBIF Backbone Taxonomy.

Это поле относится к другому классу терминов `Taxon`, и должно проверяться аудитором, если оно заполнялось оператором самостоятельно (для этого варианта должен быть предоставлен контролируемый список для подстановки значений). Сомнения в определениях заполняются в отдельное поле `identificationQualifier`.

- **`identificationQualifier`** — стандартные сокращения (aff., cf.) для обозначения сомнений специалиста в определении таксона.

- **`typeStatus`** — указание, является ли образец, записанный в этой записи, типом названия таксона.

- **`identificationRemarks`** — комментарии относительно определения таксона, включая информацию о переопределениях.

- **`identifiedBy`** — специалист, определивший находку.

**Класс `Occurrence`**

- **`recordedBy`** — кем был собран образец или сделано наблюдение.

Фамилии коллекторов или наблюдателей не всегда могут быть указаны явно и их следует поискать в разделе "Материалы и методы". Вносить неообходимо в формате `Фамилия И. О. | Фамилия И. О.`, [транслитерируя на латиницу по схеме Википедии](https://iuliia.ru/wikipedia/). Рекомендуется использовать контролируемый список для подстановки значений.

- **`catalogNumber`** — идентификатор образца в коллекции (международный акроним и номер).

Если есть дублеты, лучше потом выделить в отдельное поле `otherCatalogNumbers`.

- **`recordNumber`** — идентификатор находки, присвоенный во время её регистрации; полевой номер коллектора.

- **`associatedReferences`** — литературные источники (библиографическая ссылка, глобальный уникальный идентификатор или URI), связанные с конкретной находкой.

Возможный случай заполнения этого поля — когда в публикации находка даётся на основании цитирования другой более ранней публикации. Ещё возможно указывать здесь публикации, в которых находка была переопределена.

- **`associatedSequences`** — идентификаторы последовательностей ДНК, относящейся к этой находке.

Почти всегда, когда в публикациях используются последовательности ДНК, они опубликованы в GenBank — достаточно внести только их идентификаторы в формате `MT359934 | MT366828` (т.е. последовательности, полученные для разных генетических маркеров). В случаях, когда используются другие базы данных, это нужно отметить. Позднее, аудитор данных заменит идентификаторы на адреса вида `https://www.ncbi.nlm.nih.gov/nuccore/MT359934`.

- **`associatedTaxa`** — другие таксоны, связанные с данной находкой.

Данный термин используется для обозначения связей между конкретными встречами организма с другими таксонами.

Оператор вносит только латинские названия таксонов, проверяя их по [World Flora Online](http://www.worldfloraonline.org/). Таксоны разделяются с помощью вертикальной черты. Если есть сомнения относительно видовой принадлежности (например, в источнике записано `на берёзе`), то записывается только родовое название растения.

В стандарте Darwin Core синэкологические связи могут быть зафиксированы двумя способами. Первый — указание типа связи и названия другого таксона в поле `associatedTaxa` в формате `host: Quercus robur` или `parasite of: Melica nutans`. Второй — использование расширения Darwin Core **`Resource Relationship`**: в поле  `relatedResourceID` указывается объект отношения (названия таксонов растений), в `relationshipOfResource` — тип связи (`parasite of`, `host of`). Это более сложный вариант, требующий пост-обработки аудитором.

- **`occurrenceRemarks`** — примечания относительно находки.

Сюда можно вносить оригинальное описание субстратов. Необязательное поле, по согласованию участников проекта.

**Класс `Event`**:

- **`verbatimEventDate`** — дата в том виде, как она представлена в оригинале.

Возможны, кто-то из операторов готов заполнять поле даты в международном формате `eventDate`, но для надёжности всё равно следует в первую очередь заполнять это поле. Парсинг даты в правильный формат проводится аудитором после сбора всех данных, что значительно ускоряет общую скорость работы над проектом.

- **`habitat`** — сведения о местообитании.

Из-за очень отличающихся подходов авторов публикаций к указанию и описанию местообитаний, и отсутствии практики использования общепринятой системы местообитаний типа EUNIS, это поле следует заполнять дословно как в оригинале.

**Класс `Location`**

- **`verbatimLocality`** — оригинальное описание местонахождения (локалитета), ровно так, как оно приводится в источнике.

При заполнении этого поля возможны некоторые варианты заполнения. Можно вносить только часть, описывающую локалитет, без административных регионов и районов, если они являются современными. Если в источнике приводится старые административные подразделения, отличающиеся от современных, тогда географию стоит записать полностью. Если публикация посвящена федеральному ООПТ, и в описании находок название ООПТ опускается для сокращения места, тогда следует к каждому локалитету добавить вначале название ООПТ. Если описание локалитетов выделено в публикации в отдельный блок, а в тексте используются номера локалитетов, следует вносить сначала только номера, а после обработки всей публикации — **добавить** к номерам названия локалитетов.

Локалитеты следует вносить точно на том языке, на котором они опубликованы в источнике; важно использовать соответствующие Unicode-символы для одинаково выглядящих кириллических и латинских букв.

- **`locality`** — описание локалитета.

В отличие от `verbatimLocality`, здесь локалиты записываются только на английском языке, переводя при необходимости. Для исправления ошибок, актуализации и стандартизации описания локалитета, информация в этом поле может быть записана в изменённом виде по сравнению с оригиналом (особенно если оригинал очень устаревший), а может быть и полностью скопирована (если оригинал современный и аккуратно записан).

Если у оператора возникают сомнения по поводу правильности внесения информации, лучше пропустить и ничего не заполнять.

- **`locationRemarks`** — примечания относительно локалитета.

- **`county`** — административный район.

На английском (лучше подсмотреть в Википедии). Вносить нужно не муниципальные районы и округа, а административные районы и города областного (и т. д.) значения (подчинения).

Оператору лучше предоставить возможность выбора из выпадающего списка значений.

- **`stateProvince`** — административный регион.

На английском (лучше подсмотреть в Википедии). Оператору лучше предоставить возможность выбора из выпадающего списка значений.

- **`verbatimElevation`** — оригинально описание высоты над уровнем моря.

- **`verbatimLatitude`** и **`verbatimLongitude`** — географическая широта и долгота в том виде, как она дана в публикации (без буквенных сокращений типа `N / с. ш.` и `E / в. д.`).

Всё, что касается геопривязки, будет выполнено аудитором, с привлечением инструментов автоматизации и проверки.

## Полная таблица, поддерживаемая аудитором и/ил руководителем проекта

Перечень полей (разбиты по классам терминов DarwinCore).

Record-level:
- `language`
- `basisOfRecord`
- `bibliographicCitation`
- `sourceID`

Обязательно следует указывать языки, используемые в полях записи (определяющим, как правило, будет поле `verbatimLocality` и некоторые другие verbatim-поля).

Identification:
- `verbatimIdentification`
- `identificationQualifier`
- `typeStatus`
- `identificationRemarks`
- `identifiedBy`

Taxon:
- `acceptedNameUsage`
- `scientificName`
- `taxonRank`
- `kingdom`

Минимально необходимым является указания ранга таксонов и царства, но желательно предоставлять полную классификацию — либо в одном поле `higherClassification`, либо в наборе полей `kingdom`, `phylum`, `class`, `order`, `family`, `genus`.

Occurrence:
- `occurrenceID`
- `recordedBy`
- `catalogNumber`
- `otherCatalogNumbers`
- `recordNumber`
- `associatedReferences`
- `associatedSequences`
- `associatedTaxa`
- `occurrenceRemarks`

Обязательным является наличие уникального внутри набора `occurrenceID`.

Event:
- `verbatimEventDate`
- `eventDate`
- `eventRemarks`
- `habitat`

Location:
- `verbatimLocality`
- `locality`
- `locationRemarks`
- `county`
- `stateProvince`
- `country`
- `countryCode`
- `verbatimElevation`
- `verbatimLatitude`
- `verbatimLongitude`
- `decimalLatitude`
- `decimalLongitude`
- `coordinateUncertaintyInMeters`
- `geodeticDatum`
- `georeferencedBy`
- `georeferenceProtocol`
- `georeferenceSources`

## Шаблоны

[Сокращённая таблица для внесения (.xlsx)](Literature_data_template.xlsx)