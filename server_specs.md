```
echo -e "\n================================================================" && \
echo -e "HOSTNAME     : $(hostname)" && \
echo -e "USER         : $(whoami)" && \
echo -e "Password     : [Hidden for Security]" && \
echo -e "TOTAL RAM    : $(free -h | awk '/^Mem:/ {print $2}')" && \
echo -e "SWAP SPACE   : $(free -h | awk '/^Swap:/ {print $2}')" && \
echo -e "MODEL        : $(lscpu | grep 'Model name:' | sed 's/Model name:[ \t]*//' | uniq)" && \
echo -e "CORES        : $(lscpu | awk '/^CPU\(s\):/ {print $2}') Cores" && \
echo -e "SPEED        : $(lscpu | grep 'BIOS Model name:' | awk -F'@' '{print $2}' | sed 's/^[ \t]*//')" && \
echo -e "TYPE         : $(lscpu | awk '/^Architecture:/ {print $2}')" && \
echo -e "DRIVE PATH   : $(df -h / | awk 'NR==2 {print $1}')" && \
echo -e "TOTAL DISK   : $(df -h / | awk 'NR==2 {print $2}')" && \
echo -e "================================================================"
```
