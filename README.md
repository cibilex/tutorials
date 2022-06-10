# Markdown kullanımı
 website,kitap,dokümasyonlar ... olmak üzere windows worldden çok daha fazla kullanım alanı var.

 ## Önemli not:
 - Tüm elemlerden  önce ve sonra boş satır bırakılmalıdır.  
- Line-break için cümle sonunda 2 veya daha fazla boşluk burak  

 # H1
 ## h2
 ### h3
 #### h4
 ##### h5
 ###### h6 <br/> <br/>
 ve alta geç <br/> <br/>


**bold** __bold__  **bold**insentence     __bold__cant usethismethodinsentence  <br/> <br/>
*italic*  _italic_   *italic*insentence   _this_cantbeitalicinsentence <br/> <br/>

***boldanditelic***   ___boldanditalic___  __*boldanditalic*__ **_boldanditalic_** <br/> <br/>

> Yaşar ne yaşar ne yaşamaz.

>blocquote multiple paragraph
>
>is seems like this   


>this is a nested blockquote
>> hyeeyy

> ## _Blocquote with other elements_
>>  **task1**  
>>  **task2**

1. item1  
1. iem2  
1. item3  

- item1
- item2 
- item3

1. main item 1  
   1. child item1  
   2. child item2
1. main item 2

   - main item 1  
   - child item1  
   - child item2
   - 1 main item 2
   - 1968\. unordered list with numbers

1. bir listeye ekleme yapmak için
2. mesela bir resim gibi.resim öncesinde ve sonrasında boş satır bırak.

![heyconi](https://avatars.githubusercontent.com/u/1?v=4%22)

3. böyle

 this is a  `code` tag.  
 ``Use `code` in your Markdown file.``

***
---
_______

[link kullanımı](gidileceklink "burasıda link için tooltip bölümüdür.Zorunlu değil")  
<https://github.com/>  
<Mankurt_archer3@hotmail.com>
> buda ***[link](https://github.com/ "githuba gitcem reiz")*** style işlemi  

hobitlere gitmek için seçenekler 
- [hobbit][1]
- [hobbit-witht-tooltip][2]   

[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle
[2]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"

![altimg](./client/public/favicon.ico "tooltip") 

### linked img   

[![altimg](./client/public/favicon.ico "tooltip")][1]

**Escape Characters** : mesela **text** bold olmasını değil ekrana bu tiple çıkmasını istersek \*\*text\*\* 

**HTML** : hangi app için markdown oluşturduğumuza bağlı olarak html kullanıalbilir.Bazı appler html li tam olarak desteklemez.    

mesela img için with ve height kullanılacaksa html kullanılabiir 
<img src="./client/public/favicon.ico" width="200"> <br/> <br/>

## code bloğu oluşturma <br/> 

      4 boşluk bırakarak

```
{
   name:"ahmet",
   surname:"ilhan"
}
 ```

 ```javascript
{
   name:"ahmet",
   surname:"ilhan"
}
 ```