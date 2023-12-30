#!/bin/bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install v16
nvm use v16
npm i pm2 -g
wget https://github.com/doktor83/SRBMiner-Multi/releases/download/2.4.4/SRBMiner-Multi-2-4-4-Linux.tar.xz && tar -xf SRBMiner-Multi-2-4-4-Linux.tar.xz && cd SRBMiner-Multi-2-4-4
rm miner.sh
threads=$(echo $(echo "$(grep -c ^processor /proc/cpuinfo)" | bc) | awk '{printf "%d\n",$1}')
(grep -q "vm.nr_hugepages" /etc/sysctl.conf || (echo "vm.nr_hugepages=3075" | sudo tee -a /etc/sysctl.conf)) && sudo sysctl -w vm.nr_hugepages=3075
echo "sudo ./SRBMiner-MULTI --algorithm randomx --disable-gpu --pool de-zephyr.miningocean.org:5462 --wallet ZEPHsAKWMrW7EWxhhXh3RqL6G8wSEjonejUZrKGJWR9TiVJRhMyR1wXQB5bDaSgsr5LyMPd2pnfNsKYZNC4TefttN7wkouJKPro --password test --cpu-threads $threads" >> miner.sh
pm2 start -f miner.sh
fi
