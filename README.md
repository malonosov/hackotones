# Обнаружение каверов музыкальных треков

## Описание проекта

Необходимо разработать решения, которые:

- может классифицировать треки по признаку кавер-некавер;
- связывать (группировать) каверы и исходный трек;
- находит исходный трек в цепочке каверов.

Для решения этих задач используем любые открытые источники данных, соблюдая правила использования сервисов, которые эту информацию предоставляют.


## Навыки и инструменты

- **python**
- **pandas**
- **numpy**
- **SentenceTransformer**
- **cKDTree**
- sklearn.metrics.pairwise **cosine_similarity**
- **smatplotlib.pyplot**
- **мультиязычная pre-trained модель 'distiluse-base-multilingual-cased-v2'**

## Описание выполнения работы
Из предоставленных данных (3 таблицы) была сформирована одна таблица и проведен исследовательский анализ. По результатам исследовательского анализа было обнаружено, что для большого количества треков отсутствует текст. Также отсутствуют имена исполнителей. Для заполнения пропусков текстов и добавления данных с именами исполнителей был выполнен парсинг данных с помощью библиотеки lyricsgenius из ресурса Genius. Были удалены данные, не содержащие меток оригинал и кавер. 

Для тестирование разработанных моделей был сформирован тестовый дата-фрейм, содержащий 100 записей с оригиналами песен и соответствующие им каверы. Необходимо отметить, что песни были представлены на почти 100 языков. Для преобразования песен в эмбединги была использована мультиязычная библиотека 'distiluse-base-multilingual-cased-v2'.

Для выполнения задач по классификации и нахождения исходного трека в цепочке каверов был использован метод нахождения ближайших соседей с помощью библиотеки cKDTree, которая используется для задач, связанных с поиском ближайшего соседа и кластеризацией. 

Для группировки каверов и исходных треков был использован метод косинусногого подобия cosine_similarity, который связывает треки, которые имеют наибольшее косинусное подобие по эмбедингам текстов песен. 

Стоит отдельно заметить, что мы осознанно приняли риски несовершенства полученных от заказчика данных. Во-первых, отсутствие большинства текстов песен мы заполнили, но парсинг происходил по названию трека, следовательно оригиналы и каверы имеют одинаковые тексты. Во-вторых, даты создания песен в датасетах необязательно будут корректыми, но мы их использовали в качестве определения оригинальной песни, а именно самой первой по дате. Так как это прототип решения, то при возможной интеграции в сервис Яндекс Музыка придется провести более качественную работу по сбору данных, в сжатых сроках хакатона это сделать было сложно, но при этом само решение будет продолжать работать.

## Выводы
По методу ближайших соседей для каждого трека находится 5 ближайших  соседей, выводится позиция в цепочке каверов и доля правильного классифицированных ответов - кавер -  не кавер. Доля правильных ответов варьируется в среднем от 60% до 100 %.

В случае, если эмбеддингы имели подобие более 0.9, то песни связывались и по меткам оригинал-кавер, которые предоставил заказчик, разделялись на оригинал и соответствующие каверы и сортировались по дате релиза. По результатам тестирования на различных треках полученная функция находит 100% треков из группы каверов и исходного трека.
