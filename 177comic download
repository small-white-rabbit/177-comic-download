import requests
import parsel
import time
from lxml import etree
import os
from concurrent import futures
import threading
#http://www.177pic.info/html/2020/09/3839085.html
url =input('\n---------------------------------请输入要下载的漫画主页链接---------------------------------\n')
headers = {
'Referer': url,
'cookie': '__cfduid=dcd810234aebc7a61c22f2cba42e6a0801575427344; Pmy=; qtmhhis=2019-11-4-14-7-14%5E%5E%u6211%u72EC%u81EA%u5347%u7EA7%5E%5E%u7B2C1%u8BDD%20%u6700%u5F31%u730E%u4EBA%5E%5E9%5E%5E461786%5E%5E7384%5E%5Egedou/%5E%5Ewoduzishengji_ShG_',
'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36'}
downloadname = r'G:\漫画'
#下载并保存

def get_download(src,dirnames,filename):
    time.sleep(1)
    img = requests.get(src)
    os.chdir(dirnames)
    with open('{}'.format(filename)+ '.jpg', 'wb') as fd:
        fd.write(img.content)
    with open('快速查看.html', 'a') as fdd:
        fdd.write("<div align='center'><img src='{}.jpg'></div>".format(filename))
        fdd.close()
    return src
#获取章节下图片链接
def get_page_url(url,dirnames):
    time.sleep(1)
    resp = requests.get(url,headers=headers)
    resp.encoding = resp.apparent_encoding
    end = url.split('/')[-1]
    print('当前下载第',end,'页')
    res_page = requests.get(url)
    res_page_img = parsel.Selector(res_page.text)
    imgs = res_page_img.xpath("//*[contains(@id,'post-')]/div/div[2]/p/img/@data-lazy-src").getall()
    for src in imgs:
        #print(src)
        time.sleep(1)
        fname = src.split('/')[-1]
        filename = fname.split('.')[0]
        print(filename)
        get_download(src,dirnames,filename)

#获取所有章节列表
def get_list_url(url):
    resp = requests.get(url)
    #自动转码
    resp.encoding = resp.apparent_encoding
    html = parsel.Selector(resp.text)
    #//*[contains(@id,'post-')] 该部分表达式为通用格式，数字变化不影响定位
    list_href = html.xpath('//*[contains(@id,"post-")]/div/div[3]/a/@href').getall()
    #print(list_href)
    tempNum = len(list_href)
    title = html.xpath('//*[contains(@id,"post-")]/header/h1/text()').get()
    print(title)
    dirnames = os.path.join(downloadname, title)
    if not os.path.exists(dirnames):
       os.makedirs(dirnames)
    #从1到最终页顺序取值
    for i in range(1,tempNum):
        #拼接页面链接
        herf= url+'/'+str(i)
        get_page_url(herf,dirnames)


if __name__ == "__main__":
        t = threading.Thread(target=get_list_url(url))
        t.start()
