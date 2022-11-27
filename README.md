# This scrape some math solver websites.

1. https://www.cymath.com/
2. https://www.wolframalpha.com/
3. https://www.symbolab.com/


```
"""
%2B = +
%3D = =
%2F = /

Symbolab:
sin = /sin
(=\left(
)=\right)
* = \cdot


\=\frac{num}{denom}


"""

import requests
from bs4 import BeautifulSoup
from selenium import webdriver

GETINPUT = True

uinput = "1+1"

if(GETINPUT == True):
    uinput = input("Enter Input: ")




runWOLFRAM = True
runCYMATH = True
runSYMBOLAB = True


browser = webdriver.PhantomJS(executable_path='C:\\phantomjs-2.1.1-windows\\bin\\phantomjs.exe')


def formaturlexpr(expr, urlbase, site):
    # sin(3*x)%2B4%3D4-5*pi
    if (site == "wolfram" or site == "cymath"):
        eu1 = expr.replace('+','%2B')
        eu2 = eu1.replace('=','%3D')
        eu3 = eu2.replace('/','%2F')
        endurl = eu3
        finalurl = urlbase + endurl
        # print(finalurl)
        return finalurl
    elif (site == "symbolab"):
        eu1 = expr.replace('+','%2B')
        eu2 = eu1.replace('=','%3D')
        eu3 = eu2.replace('/','%2F')
        eu4 = eu3.replace('sin','/sin')
        eu5 = eu4.replace('(','\\left(')
        eu6 = eu5.replace(')','\\right)')
        eu7 = eu6.replace('*','\\cdot')
        endurl = eu7
        finalurl = urlbase + endurl
        # print(finalurl)
        return finalurl



site = "https://www.wolframalpha.com/input/?i="


def WolframFunction():
    sitedemo = "https://www.wolframalpha.com/input/?i=sin(x)%3D24"
    site = formaturlexpr(uinput, "https://www.wolframalpha.com/input/?i=", "wolfram")


    browser.set_window_size(1120, 550)
    browser.get(site)
    browser.save_screenshot('wolframscreenshot.png')

    html = browser.page_source
    soup = BeautifulSoup(html, 'html.parser')

    SymbolicSolution = soup.find(id="SymbolicSolution")
    imglist = SymbolicSolution.find_all('img', class_='ng-isolate-scope', alt=True)

    print("WOLFRAMALPHA")

    for item in imglist:
         # print(item.get_text())
         print(item['alt'])




def CymathFunction():
    sitedemo = "https://www.cymath.com/answer?q=sin(x)%3D24"
    site = formaturlexpr(uinput, "https://www.cymath.com/answer?q=", "cymath")


    browser.set_window_size(1120, 550)
    browser.get(site)
    browser.save_screenshot('cymathscreenshot.png')


    html = browser.page_source
    soup = BeautifulSoup(html, 'html.parser')


    stepsdivlist = soup.find_all(id="steps_div")
    itnlist = soup.find_all(class_='itn')
    katexlist = soup.find_all(class_='katex')
    listmord = soup.find_all(class_='mord mathrm')
    sollist = soup.findChildren(class_='base textstyle uncramped')
    hiddenanswers = soup.find(id="answer")

    print("CYMATH")

    hiddenanswertext = hiddenanswers.get_text()
    # print(hiddenanswertext)
    hat1 = hiddenanswertext.replace('),sequence(',' & ')
    hat2 = hat1.replace('sequence(', ' ')
    hat3 = hat2[:-1]
    hat4 = hat3.replace('PI', 'π')
    hiddenanswertext = hat4
    print(hiddenanswertext)



def SymbolabFunction():
    sitedemo = "https://www.symbolab.com/solver/step-by-step/sin%5Cleft(x%5Cright)%3D24"
    site = "https://www.symbolab.com/solver/step-by-step/sin%5Cleft(x%5Cright)%3D24"


    browser.set_window_size(1120, 550)
    browser.get(site)
    browser.save_screenshot('symbolabscreenshot.png')



    html = browser.page_source
    soup = BeautifulSoup(html, 'html.parser')


    selectablelist = soup.find_all(class_='selectable')
    solutionslist = soup.find_all(class_='solution_step_list_item')

    # print(solutionsteplist)

    print("SYMBOLAB")

    for item in solutionslist:

         print(item.get_text())






if runWOLFRAM == True:
    try:
        WolframFunction()
    except:
        print("Wolfram gave up!")

if runCYMATH == True:
    try:
        CymathFunction()
    except:
        print("Cymath gave up!")

if runSYMBOLAB == True:
    try:
        SymbolabFunction()
    except:
        print("Symbolab gave up!")
```
