**Se va a importar las funciones necesarias y abrir twitter en firefox**


```python
from selenium.webdriver.firefox.firefox_binary import FirefoxBinary

from selenium import webdriver

import time

import timeit

from getpass import getpass


binary = FirefoxBinary('C:\\Program Files\\Mozilla Firefox\\firefox.exe')

driver = webdriver.Firefox(firefox_binary=binary, executable_path=r'C:\\geckodriver.exe')

driver.implicitly_wait(3)

driver.get('https://www.twitter.com')

print('twitter se ha abierto')
```

**Siguiente introducimos nombre de usuario y contraseña para iniciar sesion**


```python
name = input('introduce el username: ')
clave = getpass('introduce la clave: ')
while True:
    try:
        driver.find_element_by_xpath('/html/body/div/div/div/div/main/div/div/div/div[1]/div/a[2]').click()
        time.sleep(2)
        driver.find_element_by_xpath('/html/body/div/div/div/div[2]/main/div/div/div[1]/form/div/div[1]/label/div/div[2]/div/input').send_keys(name)
        time.sleep(1)
        driver.find_element_by_xpath('/html/body/div/div/div/div[2]/main/div/div/div[1]/form/div/div[2]/label/div/div[2]/div/input').send_keys(clave)
        time.sleep(1)
        driver.find_element_by_xpath('/html/body/div/div/div/div[2]/main/div/div/div[1]/form/div/div[3]/div').click()
        
    except:
        print('A pasado un error. No se pudo iniciar sesion')
        break
    else:
        time.sleep(5)
        if driver.current_url != 'https://twitter.com/home':
            print('Parece que ingresaste mal tu clave, vuelve a intentarlo')
            driver.get('https://www.twitter.com')
            break
        else:
            print('Se ha iniciado sesion')
            break
        
```


```python

```

**url de hashtag y user**


```python

def Hashtagtopurl1(hashtag):
    return "https://twitter.com/search?q=(%23" + hashtag + ")%20-filter%3Alinks%20-filter%3Areplies&src=typed_query"

def Hashtagtopurl2(hashtag1, hashtag2):
    return "https://twitter.com/search?q=(%23" + hashtag1 + "%20OR%20%23" + hashtag2 + ")%20-filter%3Alinks%20-filter%3Areplies&src=typed_query"

def Hashtagurlrecent1(hashtag):
    return "https://twitter.com/search?f=live&q=(%23" + hashtag + ")%20-filter%3Alinks%20-filter%3Areplies&src=typed_query"

def Hashtagurlrecent2(hashtag1, hashtag2):
    return "https://twitter.com/search?f=live&q=(%23" + hashtag1 + "%20OR%20%23" + hashtag2 + ")%20-filter%3Alinks%20-filter%3Areplies&src=typed_query"

def HashtagUserurl(hashtag, user):
    return "https://twitter.com/search?q=(%23" + hashtag + ")%20(from%3A" + user + ")%20-filter%3Alinks%20-filter%3Areplies&src=typed_query&f=live"
    
def usertweet(tweet):
    info = tweet.split("\n")
    return info[1]

print('has guardado las funciones de url')

alltweets = []

tweetsretweets = 0

conttweets = []


```

**Aqui ingresas un archivo de texto**


```python
def openmyfile():
    filelist = []
    path = input('pon el path: ').replace('\\','/')
    try:
        filetext = open(path, "r")
        for x in filetext:
            filelist.append(x.strip())
        filetext.close()
        print('se ha guardado correctamente')
        return  filelist
    except:
        print('ingresaste mal el path')

tipo = int(input('De que es el archivo? presione: 1 si es usuarios, 2 si es hashtags, 3 si es tweets'))

if tipo == 1:
    myusers = openmyfile()
    
elif tipo == 2:
    myhashtags = openmyfile()

elif tipo == 3:
    mytweets = openmyfile()
    deseo = input('Deseas agregar estos tweets a la lista alltweets? responde solamente SI o NO: ')

    si = ['si', 'Si', 'SI', 'sI']

    if deseo in si:
        alltweets.extend(mytweets)
        alltweets = tuple(alltweets)
        alltweets = list(alltweets)
        print('Se ha agregado estos tweets a la lista "alltweets"')

else:
    print('ingresaste mal, debes solo ingresar 1, 2 o 3, vuelve a correr el codigo')





```


```python

```


```python

```

**tomar texto de publicaciones**




```python
tweets= []

print('Puedes realizar diferentes tipos de recopilacion de tweets:')
print('1) Busqueda de un hashtag en la seccion de Destacados')
print('2) Busqueda de dos hashtags en la seccion de Destacados')
print('3) Busqueda de un hashtag en la seccion de Recientes')
print('4) Busqueda de dos hashtags en la seccion de Recientes')
print('5) Busqueda de un hashtag publicados unicamente por un usuario en especifico')
print('')

hashtagopcion = int(input('Ingresa unicamente el numero de la opcion que deseas: '))

if hashtagopcion == 1:
    hashtag1 = input("Ingresa el hashtag: ")
    driver.get(Hashtagtopurl1(hashtag1[1:len(hashtag1)]))
    
if hashtagopcion == 2:
    hashtag1 = input("Ingresa el primer hashtag: ")
    hashtag2 = input("Ingresa el segundo hashtag: ")
    driver.get(Hashtagtopurl2(hashtag1[1:len(hashtag1)], hashtag2[1:len(hashtag2)]))

if hashtagopcion == 3:
    hashtag1 = input("Ingresa el hashtag: ")
    driver.get(Hashtagurlrecent1(hashtag1[1:len(hashtag1)]))

if hashtagopcion == 4:
    hashtag1 = input("Ingresa el primer hashtag: ")
    hashtag2 = input("Ingresa el segundo hashtag: ")   
    driver.get(Hashtagurlrecent2(hashtag1[1:len(hashtag1)], hashtag2[1:len(hashtag2)]))

if hashtagopcion == 5:
    hashtag1 = input("Ingresa el hashtag: ")
    user1 = input("Ingresa el usuario: ")
    driver.get(HashtagUserurl(hashtag1[1:len(hashtag1)], user1[1:len(user1)]))

    
lentweets = int(input('ingresa cuantos tweets deseas o esperas obtener: '))

print('')

i = 1
while len(tweets)<lentweets:
    try:
        tweet = driver.find_element_by_tag_name("article").text
        pathhashtag = "/html/body/div/div/div/div[2]/main/div/div/div/div[1]/div/div[2]/div/div/section/div/div/div[" + str(i) + "]/div/div/article/div/div/div/div[2]/div[2]/div[2]/div[1]"
        tweetext= driver.find_element_by_xpath(pathhashtag).text
        tweets.append(tweetext)
        username= usertweet(tweet)
        print('se guardo el tweet de ' + username)
        i+=1
        driver.execute_script("return document.getElementsByTagName('article')[0].remove();")
        e=0


    except Exception:
        try:
            driver.execute_script("return document.getElementsByTagName('a')[12].remove();")
        except:
            print('houston tenemos un problema... ')
            e+=1
            if e>5:
                print('No se pudo completar el proceso')
                break


tweetscantidad = len(tweets)
if e>5:
    print('Lo siento mucho, solo se guardaron ' + str(tweetscantidad) + ' tweets')
else:
    print('Se han guardado ' + str(tweetscantidad) + ' tweets')

deseo = input('Deseas agregar estos tweets a la lista alltweets? responde solamente SI o NO')

si = ['si', 'Si', 'SI', 'sI']

if deseo in si:
    alltweets.extend(tweets)
    alltweets = tuple(alltweets)
    alltweets = list(alltweets)
    print('Se ha agregado estos tweets a la lista "alltweets"')
    
    
```

**retweet y like**


```python
print('Puedes realizar estas siguientes acciones:')
print('A) Retweetear')
print('B) Dar like')
print('C) Ambas')

acciones = input('Responde unicamente la letra de la opcion que escojas: ')

if acciones == 'c':
    acciones = 'C' 
if acciones == 'b':
    acciones = 'B' 
if acciones == 'a':
    acciones = 'A' 

print('Puedes realizar la(s) accion(es) escogidas a:')
print('1) Los tweets de un hashtag en la seccion de Destacados')
print('2) Los tweets de dos hashtags en la seccion de Destacados')
print('3) Los tweets de un hashtag en la seccion de Recientes')
print('4) Los tweets de dos hashtags en la seccion de Recientes')
print('5) Los tweets de un usuario usando un hashtag en especifico')
print('')

hashtagopcion = int(input('Ingresa unicamente el numero de la opcion que deseas: '))

if hashtagopcion == 1:
    hashtag1 = input("Ingresa el hashtag: ")
    driver.get(Hashtagtopurl1(hashtag1[1:len(hashtag1)]))

if hashtagopcion == 2:
    hashtag1 = input("Ingresa el primer hashtag: ")
    hashtag2 = input("Ingresa el segundo hashtag: ")
    driver.get(Hashtagtopurl2(hashtag1[1:len(hashtag1)], hashtag2[1:len(hashtag2)]))

if hashtagopcion == 3:
    hashtag1 = input("Ingresa el hashtag: ")
    driver.get(Hashtagurlrecent1(hashtag1[1:len(hashtag1)]))

if hashtagopcion == 4:
    hashtag1 = input("Ingresa el primer hashtag: ")
    hashtag2 = input("Ingresa el segundo hashtag: ")   
    driver.get(Hashtagurlrecent2(hashtag1[1:len(hashtag1)], hashtag2[1:len(hashtag2)]))

if hashtagopcion == 5:
    user1 = input("Ingresa el usuario: ")
    hashtag1 = input("Ingresa el hashtag: ")
    driver.get(HashtagUserurl(hashtag1[1:len(hashtag1)], user1[1:len(user1)]))

    
lentweets = int(input('ingresa a cuantos tweets deseas o esperas realizar dicha(s) accion(es): '))

print('')

i = 1
while i-1<lentweets:
    try:
        tweet = driver.find_element_by_tag_name("article").text
        username= usertweet(tweet)
        
        if acciones == 'A' or acciones == 'C':
            pathretweet = '/html/body/div/div/div/div[2]/main/div/div/div/div[1]/div/div[2]/div/div/section/div/div/div[' + str(i) + ']/div/div/article/div/div/div/div[2]/div[2]/div[2]/div[3]/div[2]/div/div/div[1]'
            driver.find_element_by_xpath(pathretweet).click() 
            try:
                driver.find_element_by_xpath('/html/body/div/div/div/div[1]/div[2]/div/div/div/div[2]/div[3]/div/div/div/div').click()
            except:
                pass
            print('se retweeteo el tweet de ' + username)

            conttweets.append(timeit.default_timer())
            try:
                if conttweets[49]:
                    if conttweets[49] - conttweets[0] > 1799:
                        print('Has alcanzado el maximo de tweets en 30 minutos')
                        print('Podras volver a tweetear 25 tweets dentro de ' + str(((conttweets[49]-conttweets[24])//60)+1) + 'minutos')
                        if acciones == 'C':
                            acciones = 'B'
                        else:
                            conttweets.pop(0)
                            break
                    conttweets.pop(0)
            except:
                pass
                    
            time.sleep(1)
            
        if acciones == 'B' or acciones == 'C':
            pathlike = '/html/body/div/div/div/div[2]/main/div/div/div/div[1]/div/div[2]/div/div/section/div/div/div[' + str(i) + ']/div/div/article/div/div/div/div[2]/div[2]/div[2]/div[3]/div[3]/div/div/div[1]'
            driver.find_element_by_xpath(pathlike).click()
            print('se dio like al tweet de ' + username)
            time.sleep(1)

        i+=1
        driver.execute_script("return document.getElementsByTagName('article')[0].remove();")
        e=0

    except Exception:
        try:
            driver.execute_script("return document.getElementsByTagName('a')[12].remove();")
            print('houston... tenemos un problema, creo que se acabaron los tweets o hay mucha interaccion')
            e+=1
            if e>5:
                print('No se pudo completar el proceso')
                break
        except:
            print('houston tenemos un problema... ')
            e+=1
            if e>5:
                print('No se pudo completar el proceso')
                break


tweetscantidad = i-1
if e>5:
    print('Lo siento mucho, solo se han retweeteado ' + str(tweetscantidad) + ' tweets')
else:
    print('Se ha realizado la(s) accion(es) a ' + str(tweetscantidad) + ' tweets')


   
```

**tweetear**


```python
print('Puedes realizar las siguientes opciones:')
print('1) Tweetear las lineas del archivo que ingresaste')
print('2) Tweetear los tweets que guardaste de un hashtag')
opcion = int(input('Responde unicamente con el numero de la opcion que deseas realizar: '))

if opcion == 1:
    n = len(mytweets)
elif opcion == 2:
    n = len(tweets)

print('Estas a punto de realizar ' + str(n) + ' tweets.')
acuerdo = input('Estas de acuerdo de realizar esa cantidad de tweets? responde solamente SI o NO: ')

si = ['si', 'Si', 'SI', 'sI']
no = ['no', 'No', 'NO', 'nO']

if acuerdo in no:
    n= int(input('Entonces, cuantos tweets desear hacer? '))
    
print('ok, a tweetear ;)')

error=0

for i in range(n):
    alert=0
    while True:
        try:   
            time.sleep(3)
            
            driver.get('https://twitter.com/compose/tweet')
   
            time.sleep(2)
    
            if opcion == 1:
                driver.find_element_by_xpath('/html/body/div/div/div/div[1]/div[2]/div/div/div/div/div/div[2]/div[2]/div/div[3]/div/div/div/div[1]/div/div/div/div/div[2]/div[1]/div/div/div/div/div/div/div/div/div/div[1]/div/div/div/div[2]/div').send_keys(mytweets[i])

    
            if opcion == 2:
                driver.find_element_by_xpath('/html/body/div/div/div/div[1]/div[2]/div/div/div/div/div/div[2]/div[2]/div/div[3]/div/div/div/div[1]/div/div/div/div/div[2]/div[1]/div/div/div/div/div/div/div/div/div/div[1]/div/div/div/div[2]/div').send_keys(tweets[i])
    
            time.sleep(3)
            try:
                driver.find_element_by_xpath('/html/body/div/div/div/div[1]/div[2]/div/div/div/div/div/div[2]/div[2]/div/div[3]/div/div/div/div[1]/div/div/div/div/div[2]/div[4]/div/div/div[2]/div[4]').click()
                
            except:
                driver.find_element_by_xpath('/html/body/div/div/div/div[1]/div[3]/div/div/div[1]').click()
                driver.find_element_by_xpath('/html/body/div/div/div/div[1]/div[2]/div/div/div/div/div/div[2]/div[2]/div/div[3]/div/div/div/div[1]/div/div/div/div/div[2]/div[4]/div/div/div[2]/div[4]').click()
            
            time.sleep(3)
            if driver.current_url == 'https://twitter.com/home':
                print('Se ha realizado ' + str(i+1) + ' tweet')
                conttweets.append(timeit.default_timer())
                alert=1
                try:
                    if conttweets[49]:
                        if conttweets[49] - conttweets[0] > 1799:
                            print('Has alcanzado el maximo de tweets en 30 minutos')
                            print('Podras volver a tweetear 25 tweets dentro de ' + str(((conttweets[49]-conttweets[24])//60)+1) + 'minutos')
                        conttweets.pop(0)
                except:
                    pass
                break
            else:    
                print('hubo un problema, tal vez ya publicaste el mismo tweet')
                print('Modificare un poco el texto del tweet para ver si asi se arregla')
                if opcion == 1:
                    x = mytweets[i] + '.'
                    mytweets.pop(i)
                    mytweets.insert(i, x)
                    
                if opcion == 2:
                    x = tweets[i] + '.'
                    tweets.pop(i)
                    tweets.insert(i, x)

            
        except Exception:
            print('Hubo un problema')
            error+=1
            if error == 5:
                break
        
```

**finalizar:**


```python
driver.quit()
```


```python

```


```python

```


```python

```


```python

```
