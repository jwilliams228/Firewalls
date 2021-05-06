# Firewalls Assignment 

#The below scripts will accept, forward and redirect all traffic to port 8080   
sudo iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
sudo iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -i eth0 -p tcp --dport 8080 -j ACCEPT
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

#this scripts will accept MYQSL input 
sudo iptables -A INPUT -i eth0 -p tcp -m tcp --dport 3306 -j ACCEPT

#the below scripts was written to allow and block incoming SMTP, POP3 traffic.
sudo iptables -A INPUT -p tcp -m tcp --dport 25 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 465 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 110 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 995 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 143 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 993 -j ACCEPT

#script will backup firewall/iptables
sudo /sbin/iptables-save

#The command limits the rate of incoming connections per minute from a source, and it can be modified as needed. For this case, the command will prevent more than 150 connections per minute coming up from a source.
iptables -I INPUT -p tcp --dport 80 -m state --state NEW -m recent --set
iptables -I INPUT -p tcp --dport 80 -m state --state NEW -m recent --update --seconds 60 --hitcount 150 -j DROP


