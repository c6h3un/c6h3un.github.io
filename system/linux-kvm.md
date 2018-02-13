# Linux KVM

## Check service
### Check libvirt
- `$ service libvirt-bin status`
 `libvirt-bin start/running, process XXXX`
- `$ ps aux |grep libvirtd`
 `root      2354  0.0  0.0 1038448 4004 ?        Sl   Aug25  42:37 /usr/sbin/libvirtd -d`

### Check VM status

- 假設需驗證的虛擬機為 proxy，則使用指令`virsh domstate proxy`查看，如果為running 則為正常。
- 查看上面所有vm狀態，如果為running 則為正常。
`$ virsh list --all`

		Id    Name                           State
		-----------------------------------------------
		 2     web                            running
		 3     ns1                            running
		 4     proxy                          running
		 15    ubuntu                         running
		 16    redis-master                   running
		 17    redis-slave                    running
		 -     base_ubuntu                    shut off

- 如果有服務不是在running 的狀態，則使用下面指令將其開機
`$ virsh start {domain_name}`
舉例來說，我們想要開`proxy`這台虛擬機，則
`$ virsh start proxy`
4. 開啟virt-manager，進虛擬機確認開機狀態。

	```
	$ virt-manager
	```