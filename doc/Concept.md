
#  Інструменти для розгортання "міні-кластеру"

Для локального розгортання кластеру Kubernetis існують кулька інструментів.

**Minikube** — це проект Kubernetes SIGs, одне з найдосконаліших рішень на ринку. Він може розгортати кластери Kubernetes або на основі віртуальної машини (наприклад, Hyper-V, KVM2, QEMU чи інших) або в середовищі виконання контейнера (наприклад, з Docker або Podman). Завдяки уніфікованому інтерфейсу minikube є рішенням, яке не залежить від платформи. Завдяки цьому легше ділитися знаннями, надавати сценарії для автоматизації та писати документацію, яка однаково охоплює всі платформи.
Великим плюсом minikube є повна документація, вона містить не лише технічні довідники, але й довгий список навчальних посібників для багатьох конкретних випадків використання та сценаріїв розгортання.

**K3s** — це мінімізована версія Kubernetes, розроблена Rancher Labs. Вилучивши незамінні функції (застарілі, альфа-версії, плагіни за замовчуванням, вбудовані в дерево) і використовуючи полегшені компоненти (наприклад, sqlite3 замість etcd3), вони досягли значного скорочення. У результаті виходить один двійковий файл розміром близько 60 МБ. Базова система Kubernetes k3s спочатку була розроблена для IoT і периферійних обчислювальних середовищ.

**Kind** — ще один проект Kubernetes SIGs, але він значно відрізняється від minikube. Kind — це абревіатура від «Kubernetes in Docker» і народився з ідеї запустити Kubernetes у середовищі виконання контейнера (замість віртуальної машини). Як випливає з назви, він переміщує кластер у контейнери Docker. Це призводить до значно швидшої швидкості запуску порівняно з породженою ВМ. Створений насамперед для тестування Kubernetes, Kind допомагає запускати кластери Kubernetes локально та в конвеєрах CI, використовуючи контейнери Docker як «вузли».

# **Характеристики**:

|           |     Minikube | k3s  | KIND  |  
|-----------|------|-------|-------|
| Linux | amd64, arm, arm64, ppc64le, s390x  |  Linux amd64, armhf, arm64/aarch64, s390x | +  |  
| Windows | amd64  |  -- |  + |  
| Darwin | amd64, arm64  | --  | +  |  
| Podman | + | + | --|
| Monitoring | + | -- | -- |
| Automatization | + | + | + |
| Plug-ins | + | -- | -- |


## **Переваги та недоліки**:

**Minikube** - завдяки уніфікованому інтерфейсу minikube є рішенням, яке не залежить від платформи. Завдяки цьому легше ділитися знаннями, надавати сценарії для автоматизації та писати документацію, яка однаково охоплює всі платформи.
Великим плюсом minikube є повна документація, вона містить не лише технічні довідники, але й довгий список навчальних посібників для багатьох конкретних випадків використання та сценаріїв розгортання.
**K3s** - має функцію, яка виділяється, називається автоматичним розгортанням . Це дозволяє розгортати ваші маніфести Kubernetes і діаграми Helm, помістивши їх у певний каталог. K3s стежить за змінами та піклується про їх застосування без подальшої взаємодії. Це особливо корисно для конвеєрів CI та пристроїв IoT (обидва цільові варіанти використання K3). Просто створіть/оновіть свою конфігурацію, і K3s гарантує, що ваші розгортання будуть актуальними.
Крім того k3s має утіліту k3d для керування узлами k3s. K3d дозволяє командам розробників зберігати конфігурацію кластера у локальному файлі YAML, яким можна спільно користуватися командою. Цей файл містить конфігурацію для майже всіх параметрів, які складають кластер, наприклад, кількість вузлів кластера, версія Kubernetes, локально зіставлені порти, реєстри, функції та багато іншого. Завдяки цьому файлу дуже легко створювати ту саму конфігурацію кластера для команди без необхідності розробника слідувати разом із readme або сценарієм для налаштування локального кластера Kubernetes.
**Kind** дуже схожий на k3d у більшості аспектів. Так само, як k3d і minikube, ви можете встановити його за допомогою популярних менеджерів пакетів, сценаріїв і як один виконуваний файл.
Якщо ви вже знаєте, як працювати з інтерфейсом командної команди k3d, ви, ймовірно, звикнете до інтерфейсу командного рядка kind дуже швидко. Параметри майже ідентичні, як і обмеження.


## Демо

![Example](devops1.gif)

## Висновки

Визначити переможця в цьому порівнянні дуже складно. Усі три встановлені рішення, minikube, k3d і kind дуже схожі одне на одне. Є деякі плюси та мінуси будь-якого рішення, але нічого особливого. Це добре, тому що неможливо вибрати неправильний інструмент. Мені подобається загальна зручність використання всіх цих інструментів, оскільки вони призначені для професійного робочого середовища. Усі вони швидкі, прості в установці та досить прості у використанні.
Minikube трохи випереджає всі варіанти та найближче до офіційної дорожньої карти розвитку Kubernetes. Особливо для одного (потенційно недосвідченого) розробника вхідний бар’єр здається досить низьким. Тим не менш, це варіант з найбільшими вимогами до ресурсів. 
З k3s ви будете задоволені меншим споживанням ресурсів порівняно з minikube. Якщо ви працюєте в команді, файли конфігурації, які постачаються з k3d або kind, стануть величезною перевагою для всіх.
Втім у minikube є одна перевага для автоматизованих тестів - аргумент --kubernetes-version . Дуже просто встановити потрібну версію Kubernetes і вуаля, вона працює. Для використання з k3d вам потрібно переглянути відповідний образ докера k3s.