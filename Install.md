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
