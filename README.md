# Voi
Voi

*نصب پیش نیاز ها
```
sudo apt update && export DEBIAN_FRONTEND=noninteractive && sudo apt-get upgrade -y && echo OK

sudo systemctl start unattended-upgrades && sudo systemctl enable unattended-upgrades

```
کامند پایین رو زدید . اینتر کنید وسط نصب که جلو بره
```
sudo apt install -y jq bc gnupg2 curl software-properties-common
curl -o - https://releases.algorand.com/key.pub | sudo tee /etc/apt/trusted.gpg.d/algorand.asc
sudo add-apt-repository "deb [arch=amd64] https://releases.algorand.com/deb/ stable main"

```

نصب نود

```
sudo apt update && sudo apt install -y algorand && echo OK

```
نود رو استوپ کنید
```
sudo systemctl stop algorand && sudo systemctl disable algorand && echo OK

```

* ست کردن shell برای ران کردن کامند goal

```
echo -e "\nexport ALGORAND_DATA=/var/lib/algorand/" >> ~/.bashrc && source ~/.bashrc && echo OK
sudo adduser $(whoami) algorand && echo OK

```

کانفیگ نود VOI

```
sudo algocfg set -p DNSBootstrapID -v "<network>.voi.network" -d /var/lib/algorand/ &&\
sudo algocfg set -p EnableCatchupFromArchiveServers -v true -d /var/lib/algorand/ &&\
sudo chown algorand:algorand /var/lib/algorand/config.json &&\
sudo chmod g+w /var/lib/algorand/config.json &&\
echo OK

```

اگر ارور گرفتید اوکی هست برید کد بعدی

```
sudo curl -s -o /var/lib/algorand/genesis.json https://testnet-api.voi.nodly.io/genesis &&\
sudo chown algorand:algorand /var/lib/algorand/genesis.json &&\
echo OK


```

```
sudo cp /lib/systemd/system/algorand.service /etc/systemd/system/voi.service &&\
sudo sed -i 's/Algorand daemon/Voi daemon/g' /etc/systemd/system/voi.service &&\
echo OK

```

* نود رو استارت کنید

```
sudo systemctl start voi && sudo systemctl enable voi && echo OK

```
استاتوس نود رو بگیرید
```
goal node status


```

یه همچین چیزی میبینید

Last committed block: 7  
Time since last block: 1.0s  
Sync Time: 3.5s  
Last consensus protocol: https://github.com/algorandfoundation/specs/tree/abd3d4823c6f77349fc04c3af7b1e99fe4df699f  
Next consensus protocol: https://github.com/algorandfoundation/specs/tree/abd3d4823c6f77349fc04c3af7b1e99fe4df699f   
Round for next consensus protocol: 8  
Next consensus protocol supported: true  
Last Catchpoint:  
Genesis ID: voitest-v1  
Genesis hash: IXnoWtviVVJW5LGivNFc0Dq14V3kqaXuK2u5OQrdVZo=  

حواستون باشه خروجی زیر عدد های جنسیس هش و جنسیس ایدی یکی باشه با زیر

Genesis ID: voitest-v1

and

Genesis hash: IXnoWtviVVJW5LGivNFc0Dq14V3kqaXuK2u5OQrdVZo=

** برای این که سریع سینک بشیم

```
goal node catchup $(curl -s https://testnet-api.voi.nodly.io/v2/status|jq -r '.["last-catchpoint"]') &&\
echo OK
```


دوباره استاتوس بگیرید

```
goal node status


```

یه همچین چیزی میبینید

Last committed block: 33069  
Sync Time: 181.3s  
Catchpoint: 30000#33ABPU3KRJEQIX4NCTMK4CSBNXWDXSN36X47OHDKJEVW7MPSK3ZA  
Catchpoint total accounts: 33  
Catchpoint accounts processed: 33  
Catchpoint accounts verified: 33  
Catchpoint total KVs: 0  
Catchpoint KVs processed: 0   
Catchpoint KVs verified: 0  
Catchpoint total blocks: 1321  
Catchpoint downloaded blocks: 796  
Genesis ID: voitest-v1  
Genesis hash: IXnoWtviVVJW5LGivNFc0Dq14V3kqaXuK2u5OQrdVZo=  


** کامند زیر رو وارد کنید یه قسمتی داره به نام checkpoint . 
چند دقیقه صبر کنید جلوش خالی باشه
```
goal node status -w 1000

```


جای xxx اسم دلخواه بذارید مثلا crypton

```
sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry name -n XXX

```

```
sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry enable &&\
sudo systemctl restart voi

```
ساخت ولت



```
goal wallet new voi


```
رمز و وارد کنید . y بزنید که کلمات بازیابی نشون بده . یه جا زخیره کنید

** ایمپورت اکانت

```
goal account import

```
پسووردتون رو بزنید . تو لاین بعدی کلمات بازیابیتون رو کپی کنید ( حواستون باشه اسپیس اول و اخر نیوفته ارور بگیرید ) . اینتر کنید
ادرسی که بهتون رو میده رو یادداشت کنید . این ادرس ولتتونه



برید دیسکورد https://discord.com/invite/grZPFnKa عضو بشید . رول Node Runner رو اولش انتخاب کنید . تو چنل ها بخش پایین یه چنل وجود داره به نام kibisis


https://discord.com/channels/1055863853633785857/1181252381816655952  


اونجا کامند /voi-testnet-faucet رو بزنید . یه سری کامند براتون میاد . انتخابش کنید . جلوی ادرس . ادرس ولتتون که تو مرحله قبل گرفتید رو وارد کنید . یه چیزی میاد حل کنید تایید کنید . 2 بار میتونید 2000 تا توکن فاست بگیرید . میتونید همون موقع 2 تلاشتونو انجام بدید و توکن هاتونو بگیرید با همین روش

شرکت کردن در فاز نود و ثبت ادرس . کد پایین رو بزنید چند دقیقه بعد بهتون یه participation id میده

******************** حواستون باشه تو کامند های از اینجا تا اخر . هر جا ادرس ولت خواست باید ادرس ولت کپی کنید . هر جا پسوورد خواست وارد کنید 

```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
echo -ne "\nEnter duration in rounds [press ENTER to accept default)]: " && read duration &&\
start=$(goal node status | grep "Last committed block:" | cut -d\  -f4) &&\
duration=${duration:-2000000} &&\
end=$((start + duration)) &&\
dilution=$(echo "sqrt($end - $start)" | bc) &&\
goal account addpartkey -a $addr --roundFirstValid $start --roundLastValid $end --keyDilution $dilution


```
خروجی باید شبیه زیر باشه . یادداشتش کنید
Participation key generation successful. Participation ID: CJOZKSLXZUNEPPOFLRU7JPISOPRVMBJASP2EIFP6CKIKJTAIEMNA  
Generated with goal v3.17.0  


استاتوس نودتون رو با کد پایین چک کنید . ازتون میخواد ادرس ولتتون رو هم وارد کنید


```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'

```

باید بزنه شما آفلاین هستید 
You are offline. 


حالا کد پایین رو میزنیم که آنلاین شیم
```

getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account changeonlinestatus -a $addr -o=1 &&\
sleep 1 &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'


```

خروجی باید بشه یه همچین چیزی
Please enter the password for wallet 'voi':   
Transaction id for status change transaction: 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA 
Transaction 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA still pending as of round 34820  
Transaction 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA still pending as of round 34821  
Transaction 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA committed in round 34822  
You are online! 

با کامند زیر چک کنید اینبار آنلاین هستید

```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'

```


=================================== تمام



هر بار سرور رو باز میکنید و خواستید چک کنید برای این که مجدد بتونید استاتوس هارو ببینید حتما کامند زیر رو اول اجرا کنید

```
echo -e "\nexport ALGORAND_DATA=/var/lib/algorand/" >> ~/.bashrc && source ~/.bashrc && echo OK
sudo adduser $(whoami) algorand && echo OK

sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry enable &&\
sudo systemctl restart voi

```

اگر بعدا چک کردید و افلاین بودید کامند های زیر رو به ترتیب بزنید

```
sudo ALGORAND_DATA=/var/lib/algorand diagcfg telemetry enable &&\
sudo systemctl restart voi

```


```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account changeonlinestatus -a $addr -o=0 &&\
sleep 1 &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'


```

```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
echo -ne "\nEnter duration in rounds [press ENTER to accept default)]: " && read duration &&\
start=$(goal node status | grep "Last committed block:" | cut -d\  -f4) &&\
duration=${duration:-2000000} &&\
end=$((start + duration)) &&\
dilution=$(echo "sqrt($end - $start)" | bc) &&\
goal account renewpartkey -a $addr --roundLastValid $end --keyDilution $dilution


```

```
Please enter the password for wallet 'voi': 
Transaction id for status change transaction: 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA
Transaction 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA still pending as of round 34820
Transaction 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA still pending as of round 34821
Transaction 5KYUOGQYKTVPN5RBFFKDNMYUIQZY5RK5VQIMEDZDE2FPIB32M3OA committed in round 34822
You are online!


```

```
getaddress() {
  if [ "$addr" == "" ]; then echo -ne "\nNote: Completing this will remember your address until you log out. "; else echo -ne "\nNote: Using previously entered address. "; fi; echo -e "To forget the address, press Ctrl+C and enter the command:\n\tunset addr\n";
  count=0; while ! (echo "$addr" | grep -E "^[A-Z2-7]{58}$" > /dev/null); do
    if [ $count -gt 0 ]; then echo "Invalid address, please try again."; fi
    echo -ne "\nEnter your voi address: "; read addr;
    addr=$(echo "$addr" | sed 's/ *$//g'); count=$((count+1));
  done; echo "Using address: $addr"
}
getaddress &&\
goal account dump -a $addr | jq -r 'if (.onl == 1) then "You are online!" else "You are offline." end'


```

