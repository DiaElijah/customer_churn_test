# <center> Тестовое задание </center>

Вы работатете ML-инженером в онлайн-кинотеатре. Один из способов привлечения пользователей – триальный период, 7 дней бесплатного полного доступа к сервису. Далее пользователь либо покупает платную подписку, либо покидает сервис.

В файле содержатся данные о конверсии пользователей из триального периода в покупку первой подписки.

## Колонки в данных:
 - user_id: str, id пользователя;
 - start_trial_date: str, день начала триального периода пользователя;
 - source: str, источик появления пользователя на сервисе;
 - device: str, устройство, с которого пользователь зарегистрировался;
 - city: str, город, из которого пользователь зарегистрировался;
 - favourite_genre: str, любимый жанр, указанный пользователем при регистрации;
 - avg_min_watch_daily: float, среднее число минут просмотра в день в течение триального периода;
 - number_of_days_logged: int, количество дней в течение триального пероида, когда пользователь логировался на сервисе;
 - churn: int, купил ли пользователь подписку по окончании триального периода (0) или нет (1).

Требуется на основе этих данных построить модель предсказания оттока пользователей онлайн-видеостриминга.

## Какие шаги хотелось бы видеть в отчёте:
1. EDA: 
  - распределение значений признаков;
  - взаимосвязь признаков друг с другом и целевой переменной;
2. Подготовка данных: 
  - обработка категориальных признаков;
  - обработка числовых признаков;
  - преобразование дат;
  - обработка пустых значений и выбросов;
3. Обучение модели: 
  - проработка схемы валидации;
  - обучение бустинга;
  - обучение нейросети;
4. Сравнение качества работы обученного бустинга и нейросети.

### Выводы

В исходном датасете заполнены пропуски в признаках город и любимый жанр. Пропущенные значения любимого жанра заполнены модой с привязкой к городу. Также обработан признак дня начала триального периода пользователя, разбиением этого признака на 3 новых признака год, месяц, день.

Проведена разведка данных, которая показала, что с таргетом коррелирует только среднее число минут просмотра в день в течение триального периода. Также разведка данных покказала, что таргет имеет классовый дизбаланс. Задача представляет собой бинарную классификацию, в связи с этим лучше использовать ROC_AUC, f1_score, PR_AUC в качестве метрик для оценки модели.
  
Статистические тесты показали, что статистически значимыми признаками являются avg_min_watch_daily - среднее число минут просмотра в день в течение триального периода, number_of_days_logged - количество дней в течение триального пероида, когда пользователь логировался на сервисе.  
  
Сформировано 2 набора обучающих и валидационных данных:  
- с полным набором данных  
- с набором статистически значимых данных  
Данных закодированы TargetEncoder-от и отскалированы.
  
Обучены классические ML модели LogReg, RandomForest, CatBoost. Практика показала, что статистичесикм тестам можно доверять. Лучшие результаты показала линейная модель, что говорит о отсутствии сложных нелинейных связей. Проведена оптимизация LogReg. На валидационной выборке после оптимизации значение ROC AUC = 0.814.
Бустинг также можно было оптимизировать и достичь значение ROC AUC = 0.814, но в этом нет смысла, т.к. LogReg работает и обучается быстрее при равном качестве.
  
Построена и обучена однослойная нейронная сеть. Как и следовало ожидать, применение нейронной сети не привело к улучшению метрики, она также приобретает значение ROC AUC = 0.814 на 6-8 эпохах обучения.
