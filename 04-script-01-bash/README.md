hugin### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-01-bash/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

---


# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

## Обязательная задача 1

Есть скрипт:
```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- | ------------- | ------------- |
| `a`  | `a+b`  | Переменной `с` присваивается значение строка `a+b`, т.к. `a` и `b` здесь это текст, а не переменные. |
| `b`  | `1+2`  | Переменной `d` присваивается значение строка, состоящая из значения переменной `a` (1), символа `+` и значения переменной `b` (2), т.к. по умолчанию Bash интерпритирует значения переменных как строковые, даже если они состоят только из цифр. |
| `c`  | `3`  | Переменной `е` присваивается значение `3`, т.к. используется форма `$((...))`, внутри которой выполняется арифметическая операция со значениями переменных `a` и `b`. |


## Обязательная задача 2
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

**Ответ:**
1. В условии цикла ``` while ``` пропущена закрывающая скобка;
2. Для того, чтобы место на жестком диске уменьшалось медленнее, введем для проверок период, например 60 секунд, при помощи команды ```sleep```;
3. Чтобы, как только сервис станет доступным, выполнение скриптазавершилось, необходимо в условный оператор ```if``` добавить условие выхода из цикла: ```else exit```

Исправленный скрипт:
```bash
while ((1==1))
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	else exit
	fi
	sleep 60
done
```

Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
ip_arr=(192.168.0.1 173.194.222.113 87.250.250.24)
for i in {1..5}
do
date >>ip.log
    for ip in ${ip_arr[@]}
    do
        curl -s --connect-timeout 1 $ip:80 >/dev/null
        echo "IP:" $ip state=$? >>ip.log
    done
done
```

## Обязательная задача 3
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
ip_arr=(192.168.0.1 173.194.222.113 87.250.250.24)
state=0
while (($state == 0))
do
	for ip in ${ip_arr[@]}
	do
		curl -s --connect-timeout 1 $ip:80 >/dev/null
		state=$?
		if (($state != 0))
		then        
			echo $ip state=$? "ERROR" >>error.log
		fi
	done
done
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: \[04-script-01-bash\] сломал хук.

### Ваш скрипт:
```bash
???
```
