# Hostowanie aplikacji .NET 5 na urządzeniu Raspberry PI
Zapraszam do obejrzenia materiału przedstawiającego jak za pomocą RaspberryPI oraz systemu RaspbianOS wyhostować aplikację stworzoną w ASP.NET MVC.
Link do filmu [Hostowanie aplikacji utworzonej w .NET 5 na RaspberryPI](https://www.google.com)

## Uploadowanie aplikacji .NET 5
Kod aplikacji znajdziesz w katalogu **src**  
1. Przejdź do katalogu **DotnetOnLinux.Web** i wykonaj poniższe polecenie  
`dotnet publish -r linux-arm -o output`
2. Uruchom WSL ([Instrukcja instalacji](https://docs.microsoft.com/en-us/windows/wsl/install-win10)) i przekopiuj plik do RaspberryPI  
`scp -r output/ pi@raspberrypi:/home/pi/app/`

## Instalacja .NET 5 na RasbperryPI
Skrypt instalacyjny  
`curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel Current`  
Tworzenie ścieżek do polecenia dotnet  
`echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc`  
`echo 'export PATH=$PATH:$HOME/.dotnet' >> ~/.bashrc`  
`source ~/.bashrc`  

## Instalacja Certbota i generowanie certyfikatu
`sudo apt-get install certbot`  
`sudo certbot certonly --standalone -d <<NAZWA_DOMENY>>`

## Tworzenie serwisu dla aplikacji ASP.NET MVC
1. Wykonaj poniższe polecenie uzupełniając nazwę serwisu:  
`sudo nano /etc/systemd/system/<<service_name>>.service`  
Zawartość konfiguracji przekopiuj z pliku **service_code**.
2. Zaktywuj serwis poleceniem `sudo systemctl enable dotnet-on-linux.service`
3. Uruchom serwis oraz sprawdź jego status:  
`sudo systemctl start <<service_name>>.service`  
`sudo systemctl start <<service_name>>.service`

## Konfiguracja Nginx
1. Wykonaj poniższe polecenie uzupełniając nazwę domeny:  
`nano /etc/nginx/sites-available/<<NAZWA_DOMENY>>.conf`  
Zawartość konfiguracji skopiuj z pliku **nginx_code** odpowiednio go wypełniając.
2. Wykonaj poniższe polecenie uzupełniając nazwę domeny:  
`sudo ln -s /etc/nginx/sites-available/<<NAZWA_DOMENY>>.conf /etc/nginx/sites-enabled/<<NAZWA_DOMENY>>.conf`
3. Zresetuj Nginxa: `sudo systemctl restart nginx`

## Podsumowanie
Na filmie zostało przedstawione jak hostować aplikację napisaną w .NET 5 za pomocą RaspberryPI przy użyciu Nginxa oraz Certbota. Pamiętaj o odpowiednim otwarciu portów na swoim routerze. Musisz również posiadać zewnętrzne IP, w przeciwnym razie nie będziesz w stanie opublikować aplikacji. Powyższe instrukcje można wykonać na dowolnym systemie bazującym na debianie.
