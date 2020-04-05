# MNST-API
Краткое описание методов
1) /userstake  - Поиск делегированных стейков по адресу кошелька в стейках указанного делегатора (может использоваться как с параметром height=, так и без)
http://95.216.6.249:8841/userstake?address=_&pub_key=_&height=_

2) /userstakes - Поиск всех делегированных стейков по адресу кошелька (может использоваться как с параметром height=, так и без)
http://95.216.6.249:8841/userstakes?address=_&height=_

3) /coinstake - Поиск всех делегированных стейков в конкретной монете по конкретной ноде (может использоваться как с параметром height=, так и без)
http://95.216.6.249:8841/coinstake?symbol=_&pub_key=_&height=_

4) /coinstakes - Поиск всех делегированных стейков в конкретной монете (может использоваться как с параметром height=, так и без)
http://95.216.6.249:8841/coinstakes?symbol=_&height=_

5) /userbalance - подробный баланс кошелька с учетом количества и стоимости всех имеющихся и делегированных монет во все ноды (может использоваться как с параметром height=, так и без)
http://95.216.6.249:8841/userbalance?address=_&height=_

6) /mcandidate - подробная информация по кандидату(в т.ч. и по валидаторам, может использоваться как с параметром height=, так и без): общий стейк, комиссия, количество занятых слотов, количество уникальных пользователей, минимальный стейк, статус кандидата.
http://95.216.6.249:8841/mcandidate?pub_key=_&height=_

7) /mcandidates - запрос аналогичный /mcandidate, но по всем кандидатам
http://95.216.6.249:8841/mcandidates?height=_

8) /convertmx - простенький конвертер кошелька стандартного вида в побайтный вариант
http://95.216.6.249:8841/convertmx?wlt=_

9) /mevents - поиск eventов по кошельку или PublicKey валидатора, или по тому и другому в конкретном блоке
/mevents?height=_&find_ui=["wallet address or node PublicKey","wallet address or node PublicKey"...]

10) /revents - Информация по всем ревардам начисленным по конкретному блоку в разрезе валидатора и типов ревардов
/revents?height=_


	Инструкция по созданию и компиляции ноды с методами API:
	Для Ubuntu/Debian
	0) Предварительное
		sudo apt-get update
		sudo apt-get upgrade -y
	1) Установить golang и сопутствующий софт:
    		wget https://dl.google.com/go/go1.13.8.linux-amd64.tar.gz
    		tar -C /usr/local -xvzf go1.13.8.linux-amd64.tar.gz
    		mkdir -p ~/go/{bin,src,pkg}
    		nano ~/.profile
        		+++
            			export PATH=$PATH:/usr/local/go/bin
            			export GOPATH="$HOME/go"
            			export GOBIN="$GOPATH/bin"
        		+++
    		source ~/.profile
    		apt-get install go-dep
    		apt-get install build-essential
    		apt-get install git
    		apt-get install screen
    		apt-get install zip
	2) Установить максимальное количества открытых файлов для всех пользователей:
    		nano /etc/security/limits.conf 
        		+++
        			root hard nproc 500000
        			root soft nproc 500000
        			root hard nofile 500000
        			root soft nofile 500000
        		+++
    		reboot
    	3) выгружаем исходники ноды Minter
    		mkdir -p $GOPATH/src/github.com/MinterTeam
    		cd $GOPATH/src/github.com/MinterTeam
    		git clone https://github.com/MinterTeam/minter-go-node/
    
	4) Как компилировать бинарник из исходников(если компилировать без правок, то получится бинарник аналогичный официальному релизу на https://github.com/MinterTeam/minter-go-node):
    		cd $GOPATH/src/github.com/MinterTeam/minter-go-node
    		go build -o ~/minter ./cmd/minter/
	5) Расширение API:
    		В папку $GOPATH/src/github.com/MinterTeam/minter-go-node/api выгружаем файлы:
        		wget https://github.com/MNSTteam/MNST-API/archive/master.zip
        		unzip master.zip coinstake.go coinstakes.go convertmx.go mcandidate.go mcandidates.go mevents.go revents.go userbalance.go userstake.go userstakes.go -d $GOPATH/src/github.com/MinterTeam/minter-go-node/api
   	
		Прописываем модули в файле api.go:
        		nano $GOPATH/src/github.com/MinterTeam/minter-go-node/api/api.go
            			+++
            				"userstakes":             rpcserver.NewRPCFunc(Userstakes, "height,address"),
	            			"userstake":              rpcserver.NewRPCFunc(Userstake, "pub_key,height,address"),
	            			"coinstakes":             rpcserver.NewRPCFunc(Coinstakes, "height,symbol"),
	            			"coinstake":              rpcserver.NewRPCFunc(Coinstake, "pub_key,height,symbol"),
	            			"userbalance":            rpcserver.NewRPCFunc(Userbalance, "height,address"),
					"mcandidate":             rpcserver.NewRPCFunc(mCandidate, "pub_key,height"),
	            			"mcandidates":            rpcserver.NewRPCFunc(mCandidates, "height"),
	            			"mevents":                rpcserver.NewRPCFunc(mEvents, "height,find_ui"),
	            			"convertmx":              rpcserver.NewRPCFunc(MXconvert, "wlt"),
	            			"revents":                rpcserver.NewRPCFunc(rEvents, "height"),
            			+++
        
        	Cобираем бинарник:
			cd $GOPATH/src/github.com/MinterTeam/minter-go-node
        		go build -o ~/minter ./cmd/minter/
        
        	Запускаем ноду с новыми методами API:
        		screen -d -m -S minter ~/minter node
         
        
    
PS: Minter Address for donates Mx61d52dea150beb1de2a1bd24c556d2750534b5e4
