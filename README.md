# Ilyas
# Подключение модулей
import random
import re

  #Процедура генерации пароля
def pass_gen(length):
    digits = '1234567890'
    leters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    leters_2 = 'abcdefghijklmnopqrstuvwxyz'
    symbols = '!@#$%^&*()-+'
    password = ''
    var = [digits, leters, leters_2, symbols]
    if length < 12:
        return print('Ошибка! Пароль должен иметь не менее 12 символов')
    else:
        password+=random.choice(digits)
        password+=random.choice(leters)
        password+=random.choice(leters_2)
        password+=random.choice(symbols)
        while len(password) < length:
            password+=random.choice(var[random.randint(0,3)])
        return password

def email_gen(list_of_names):
    emails = []
    for i in list_of_names:
        letter = 1
        while i[1] + '.' + i[0][0:letter] + '@company.io' in emails:
            letter+=1
        emails.append(i[1] + '.' + i[0][0:letter] + '@company.io')
    return emails


strings = []
failed = []
list_of_names = []
n = 0
f = open('task_file.txt', 'r')
for i in f:
    strings.append(i.split(','))
f.close()


for i in range(len(strings)):
    for j in range(len(strings[i])):
        strings[i][j]=strings[i][j].replace(' ','')
        
        
for i in range(len(strings)):
    if re.findall(r'^$|\W|\d|NAME', strings[i][1]) or re.findall(r'[wrtplkhgfdszxcvbnm][wrtplkhgfdszxcvbnm][wrtplkhgfdszxcvbnm][wrtplkhgfdszxcvbnm]', strings[i][1]) or re.findall(r'^$|\W|\d|NAME', strings[i][2]) or re.findall(r'0......|\D|^$', strings[i][3]) or len(strings[i][3]) != 7 or re.findall(r'\W+\n$|\d|^$', strings[i][4]):
        failed.append(i)
    else:
        list_of_names.append([strings[i][1],strings[i][2]])
emails = email_gen(list_of_names)
f_1 = open('task_file.txt', 'w')
f_2 = open('task_file_1.txt', 'w')

f_1.write('EMAIL, PASSWORD, LAST_NAME, TEL, CITY\n')
for i in range(len(strings)):
    if i not in failed:
        f_1.write(emails[n]+', '+pass_gen(12)+', '+strings[i][1]+', '+strings[i][2]+', '+strings[i][3]+', '+strings[i][4])
        n+=1
    else:
        f_2.write(strings[i][1]+', '+strings[i][2]+', '+strings[i][3]+', '+strings[i][4])
f_1.close()
f_2.close()
