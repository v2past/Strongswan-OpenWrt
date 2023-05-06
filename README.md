docker run     --name ipsec-vpn-server     --restart=always    --env-file /mnt/mmcblk2p4/docker/vpn.env    -v ikev2-vpn-data:/etc/ipsec.d    -v /lib/modules:/lib/modules:ro     -p 500:500/udp     -p 4500:4500/udp     -d --privileged   hwdsl2/ipsec-vpn-server


opkg update
opkg install strongswan-minimal strongswan-mod-eap-mschapv2 strongswan-mod-eap-identity strongswan-mod-constraints strongswan-mod-md5 strongswan-mod-pem strongswan-mod-pkcs1 strongswan-mod-revocation



https://openwrt.org/docs/guide-user/services/vpn/strongswan/site2site

#/etc/config/ipsec
config 'ipsec'
  list listen ''
  
config 'remote' 'acme'
  option 'enabled' '1'
  option 'gateway' '7.7.7.7'
  option 'pre_shared_key' 'yourpasswordhere'
  option 'exchange_mode' 'aggressive'
  option 'local_identifier' 'bratwurst'
  list   'p1_proposal' 'pre_g2_aes_sha1'
  list   'tunnel' 'acme_dmz'
  list   'tunnel' 'acme_lan'

config 'p1_proposal' 'pre_g2_aes_sha1'
  option 'encryption_algorithm' 'aes128'
  option 'hash_algorithm' 'sha1'
  option 'dh_group' '2'

config 'tunnel' 'acme_lan'
  option 'local_subnet' '192.168.2.64/26'
  option 'remote_subnet' '10.1.2.0/24'
  option 'p2_proposal' 'g2_aes_sha1'

config 'tunnel' 'acme_dmz'
  option 'local_subnet' '192.168.2.64/26'
  option 'remote_subnet' '66.77.88.192/26'
  option 'p2_proposal' 'g2_aes_sha1'

config 'p2_proposal' 'g2_aes_sha1'
  option 'pfs_group' '2'
  option 'encryption_algorithm' 'aes128'
  option 'authentication_algorithm' 'sha1'
...
