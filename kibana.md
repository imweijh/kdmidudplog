### timelion 通达信委托耗时max-avg比对（放在一行）
```
.es(index="logstash-*",q='"L0303001"',metric='avg:lbmtime',offset=-7d).label(7天前委托).color(yellow).trim(start=2,end=1).yaxis(2), 
.es(index="logstash-*",q='"L0303001"',metric='avg:lbmtime').label(L0303001).color(#00ffdf).trim(start=2,end=1).yaxis(2),  
.es(index="logstash-*",q='"L0303001"',metric='max:lbmtime').points().label(MAX委托).color(red)
```

### timelion 通达信登录耗时max-avg比对（放在一行）
```
.es(index="logstash-*",q='"L0409101"',metric='avg:lbmtime',offset=-7d).label(7天前登录).color(yellow).trim(start=2,end=1).yaxis(2), 
.es(index="logstash-*",q='"L0409101"',metric='avg:lbmtime').label(L0409101).color(#00ffdf).trim(start=2,end=1).yaxis(2),  
.es(index="logstash-*",q='"L0409101"',metric='max:lbmtime').points().label(MAX登录).color(red)
```


![image](https://user-images.githubusercontent.com/23710675/117419580-564b8880-af4f-11eb-94cc-0b21b69c41c7.png)
![image](https://user-images.githubusercontent.com/23710675/117421570-3a48e680-af51-11eb-8754-220e3bd3d5b6.png)
![image](https://user-images.githubusercontent.com/23710675/117422692-7b8dc600-af52-11eb-8f6f-ac9c063cbfd4.png)
![image](https://user-images.githubusercontent.com/23710675/117423501-62394980-af53-11eb-89ff-dda9c979b574.png)
![image](https://user-images.githubusercontent.com/23710675/117423662-8dbc3400-af53-11eb-803f-6c735ee327a7.png)
![image](https://user-images.githubusercontent.com/23710675/117423783-a75d7b80-af53-11eb-9b8f-6a8700086243.png)
![image](https://user-images.githubusercontent.com/23710675/117423967-dbd13780-af53-11eb-990b-3b2a4adfeebd.png)
![image](https://user-images.githubusercontent.com/23710675/117424284-366a9380-af54-11eb-8726-1d0ed4c4409d.png)




