Advantech 技術支援團隊您好！

我是 顏瑋良，來自 ITRI。 近期在使用 Advantech iDAQ-801 設備進行振動訊號擷取的過程中，我們遇到了一些技術難題，特此尋求貴團隊的專業協助。

我們的應用場景是收集和分析振動訊號資料。 在使用 iDAQ-801 設備進行振動資料擷取時，我們注意到所獲得的訊號與預期有差異。 為了進一步驗證，我們使用了 NI 的資料擷取卡進行了交叉對比，發現 iDAQ-801 收集到的資料與NI未開啟 IEPE (Integrated Electronics Piezo-Electric) 功能時的訊號相似。

根據我們對 iDAQ-801 產品規格的了解，該設備支援「直接 IEPE 供電」。 我們在產品提供的函數庫中找到了與 IEPE 相關的函數，推測雖然設備支援直接供電，但可能存在某種形式的「開關」需要啟動或配置。

為了解決這個問題，我們嘗試了多種程式設計方法來開啟 IEPE 功能，但遺憾的是，我們的嘗試都未能成功。 在此，我附上了我們測試時使用的部分程式碼片段，希望能獲得團隊的指導：

- 測試過使用IepeType.IEPE4mA.value
- 測試過使用IepeType.IEPE4mA
- 測試過使用int(1)
- 三個方式的執行結果都如下截圖
```=python

    for channel in range(startChannel, startChannel + channelCount):
        ai_channel = wfAiCtrl.channels[channel]
        print(f"ai_channel type: {type(ai_channel)}") 
        aii_channel = AnalogInputChannel(ai_channel)
        print(f"aii_channel type: {type(aii_channel)}") 
        print(f'可選IepeType:{list(IepeType)}')

        try:
            desired_iepe_type = IepeType.IEPE4mA.value  
            aii_channel.iepeType = desired_iepe_type
            
        except Exception as e:
            print(f'Error setting IEPE type for channel {channel}: {e}')
```

```=python

    for channel in range(startChannel, startChannel + channelCount):
        ai_channel = wfAiCtrl.channels[channel]
        print(f"ai_channel type: {type(ai_channel)}") 
        aii_channel = AnalogInputChannel(ai_channel)
        print(f"aii_channel type: {type(aii_channel)}") 
        print(f'可選IepeType:{list(IepeType)}')

        try:
            desired_iepe_type = IepeType.IEPE4mA  
            aii_channel.iepeType = int(1)
            
        except Exception as e:
            print(f'Error setting IEPE type for channel {channel}: {e}')
```

```=python

    for channel in range(startChannel, startChannel + channelCount):
        ai_channel = wfAiCtrl.channels[channel]
        print(f"ai_channel type: {type(ai_channel)}") 
        aii_channel = AnalogInputChannel(ai_channel)
        print(f"aii_channel type: {type(aii_channel)}") 
        print(f'可選IepeType:{list(IepeType)}')

        try:
            aii_channel.iepeType = int(0)
            
        except Exception as e:
            print(f'Error setting IEPE type for channel {channel}: {e}')
```

![image](https://hackmd.io/_uploads/rJoER2hu6.png)

- 兩次振動訊號不是同一筆，但一個是有開啟IEPE一個是未開啟

|開啟|未開啟|
|:--:|:--:|
|![messageImage_1704936197754](https://hackmd.io/_uploads/H1-cLa2_6.jpg)|![messageImage_1704936190487](https://hackmd.io/_uploads/HkdF8pndp.jpg)|