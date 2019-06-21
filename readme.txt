please read me
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
重烧整个系统
tftp 30000000 uImage
nand erase kernel
nand write.jffs2 30000000 kernel

tftp 30000000 fs_qtopia.yaffs2
nand erase root
mtd
nand write.yaffs 30000000 0x00260000 $(filesize)



重新挂载新的文件系统fs.yaffs2
nfs 30000000 192.168.1.101:/work/nfs_root/tmp/fs.yaffs2
nand erase root
nand write.yaffs 30000000 0x00260000 $(filesize)

使用flash上的根文件系统启动后，手工mount NFS
将虚拟机上的/work/nfs_root目录挂载到 /mnt
mount -t nfs -o nolock,vers=2 192.168.1.101:/work/nfs_root /mnt

使用nfs作为根文件系统来启动
nfsroot.txt
nfsroot=[<server-ip>:]<root-dir>[,<nfs-options>]
ip=<client-ip>:<server-ip>:<gw-ip>:<netmask>:<hostname>:<device>:<autoconf>

set bootargs noinitrd root=/dev/nfs nfsroot=192.168.1.102:/work/nfs_root/tmp/fs_mini_mdev ip=192.168.1.10:192.168.1.102:192.168.1.1:255.255.255.0::eth0:off init=/linuxrc console=ttySAC0


开发板原有第0课第7节00:11:47
bootargs=noinitrd root=/dev/mtdblock3 init=/linuxrc console=ttySAC0


make
arm-linux-gcc -o firstdrvtest firstdrvtest.c
insmod first_drv.ko
./firstdrvtest 

./firstdrvtest on

./firstdrvtest off


"# cd first_drv/
# ls
Makefile         first_drv.ko     first_drv.o
Module.symvers   first_drv.mod.c  firstdrvtest
first_drv.c      first_drv.mod.o  firstdrvtest.c
# insmod firstdrv.ko
insmod: /lib/modules/2.6.22.6: No such file or directory
insmod: /lib/modules: No such file or directory
insmod: firstdrv.ko: no module by that name found
# insmod first_drv.ko
# ./firstdrvtest
Usage :
./firstdrvtest <on|off>
# on
-sh: on: not found
# ./firstdrvtest on
# ./firstdrvtest off
# ./firstdrvtest on
# ./firstdrvtest off"

win7和win10系统无法使用dnw的替代方法
1.网络下载
2.用linux下的dnw下载
重要第0课第8节


make 100ask24x0_config

@$(MKCONFIG) $(@:_config=) arm arm920t 100ask24x0 NULL s3c24x0


MKCONFIG	:= $(SRCTREE)/mkconfig

mkconfig 100ask24x0_config arm arm920t 100ask24x0 NULL s3c24x0


BOARD_NAME


$(obj)u-boot:		depend version $(SUBDIRS) $(OBJS) $(LIBS) $(LDSCRIPT)
		UNDEF_SYM=`$(OBJDUMP) -x $(LIBS) |sed  -n -e 's/.*\(__u_boot_cmd_.*\)/-u\1/p'|sort|uniq`;\
		cd $(LNDIR) && $(LD) $(LDFLAGS) $$UNDEF_SYM $(__OBJS) \
			--start-group $(__LIBS) --end-group $(PLATFORM_LIBS) \
			-Map u-boot.map -o u-boot


UNDEF_SYM=`arm-linux-objdump -x lib_generic/libgeneric.a board/100ask24x0/lib100ask24x0.a cpu/arm920t/libarm920t.a cpu/arm920t/s3c24x0/libs3c24x0.a lib_arm/libarm.a fs/cramfs/libcramfs.a fs/fat/libfat.a fs/fdos/libfdos.a fs/jffs2/libjffs2.a fs/reiserfs/libreiserfs.a fs/ext2/libext2fs.a net/libnet.a disk/libdisk.a rtc/librtc.a dtt/libdtt.a drivers/libdrivers.a drivers/nand/libnand.a drivers/nand_legacy/libnand_legacy.a drivers/usb/libusb.a drivers/sk98lin/libsk98lin.a common/libcommon.a |sed  -n -e 's/.*\(__u_boot_cmd_.*\)/-u\1/p'|sort|uniq`;\



                cd /work/system/u-boot-1.1.6 && 


arm-linux-ld -Bstatic -T /work/system/u-boot-1.1.6/board/100ask24x0/u-boot.lds -Ttext 0x33F80000  $UNDEF_SYM cpu/arm920t/start.o \
                        --start-group lib_generic/libgeneric.a board/100ask24x0/lib100ask24x0.a cpu/arm920t/libarm920t.a cpu/arm920t/s3c24x0/libs3c24x0.a lib_arm/libarm.a fs/cramfs/libcramfs.a fs/fat/libfat.a fs/fdos/libfdos.a fs/jffs2/libjffs2.a fs/reiserfs/libreiserfs.a fs/ext2/libext2fs.a net/libnet.a disk/libdisk.a rtc/librtc.a dtt/libdtt.a drivers/libdrivers.a drivers/nand/libnand.a drivers/nand_legacy/libnand_legacy.a drivers/usb/libusb.a drivers/sk98lin/libsk98lin.a common/libcommon.a --end-group -L /work/tools/gcc-3.4.5-glibc-2.3.6/lib/gcc/arm-linux/3.4.5 -lgcc \
                        -Map u-boot.map -o u-boot



grep "33f80000" * -nR


下次第9课第3节


下次第9课第4节
argv[0] = "md.w"
argv[1] = "0"


#define Struct_Section  __attribute__ ((unused,section (".u_boot_cmd")))

bootcmd=nand read.jffs2 0x30007FC0 kernel; 

bootm 0x30007FC0

U_BOOT_CMD(
 	bootm,	CFG_MAXARGS,	1,	do_bootm,
 	"bootm   - boot application image from memory\n",
 	"[addr [arg ...]]\n    - boot application image stored in memory\n"
 	"\tpassing arguments 'arg ...'; when booting a Linux kernel,\n"
 	"\t'arg' can be the address of an initrd image\n"
#ifdef CONFIG_OF_FLAT_TREE
	"\tWhen booting a Linux kernel which requires a flat device-tree\n"
	"\ta third argument is required which is the address of the of the\n"
	"\tdevice-tree blob. To boot that kernel without an initrd image,\n"
	"\tuse a '-' for the second argument. If you do not pass a third\n"
	"\ta bd_info struct will be passed instead\n"
#endif
);



#define U_BOOT_CMD(name,maxargs,rep,cmd,usage,help) \
cmd_tbl_t __u_boot_cmd_##name Struct_Section = {#name, maxargs, rep, cmd, usage, help}


cmd_tbl_t __u_boot_cmd_bootm __attribute__ ((unused,section (".u_boot_cmd"))) = 
{"bootm", CFG_MAXARGS, 1, do_bootm, usage, help}



nand read.jffs2 0x30007FC0 kernel; 
从nand读出内核：从哪里读，
                放到哪里去？——0x30007FC0

bootm 0x30007FC0


#define MTDPARTS_DEFAULT "mtdparts=nandflash0:256k@0(bootloader)," \
                            "128k(params)," \
                            "2m(kernel)," \
                            "-(root)"

nand read.jffs2 0x30007FC0 0x00060000 0x00200000
      0x00060000


nand read[.jffs2]     - addr off|partition size


u-boot 
Flash上存的内核:
uImage
头部+真正的内核


	uint32_t	ih_load;	/* Data	 Load  Address		*/
	uint32_t	ih_ep;		/* Entry Point Address		*/

bootm:
1.读取头部移动内核到合适的地方去；
2.启动do_bootm_linux


theKernel


setup_start_tag (bd);
setup_memory_tags (bd);
setup_commandline_tag (bd, commandline);
setup_end_tag (bd);


bootcmd=nand read.jffs2 0x30007FC0 kernel; bootm 0x30007FC0
bootdelay=2
baudrate=115200
ethaddr=08:00:3e:26:0a:5b
netmask=255.255.255.0
mtdids=nand0=nandflash0
mtdparts=mtdparts=nandflash0:256k@0(bootloader),128k(params),2m(kernel),-(root)
ipaddr=192.168.1.10
serverip=192.168.1.100
bootargs=noinitrd root=/dev/nfs nfsroot=192.168.1.101:/work/nfs_root/tmp/fs_mini_mdev ip=192.168.1.10:192.168.1.101:192.168.1.1:255.255.255.0::eth0:off init=/linuxrc console=ttySAC0
stdin=serial
stdout=serial
stderr=serial
partition=nand0,0
mtddevnum=0
mtddevname=bootloader


theKernel (0, bd->bi_arch_number, bd->bi_boot_params);


find -name "defconfig"

find -name "*defconfig*"


配置：
a.make menuconfig

b.使用默认配置，在上面修改
cd ././arch/arm/configs/
在arch/arm/configs/目录下找到相似的配置文件xxx_defconfig
然后make xxx_defconfig
如

make s3c2410_defconfig  保存到.config
然后
make menuconfig

c.使用厂家提供的配置文件
cp config_厂家 .config

cp config_ok .config

make menuconfig

改变环境变量
set bootargs noinitrd root=/dev/mtdblock3 init=/linuxrc console=ttySAC0

宏 CONFIG_DM9000
grep "CONFIG_DM9000" * -nwR



视频10.2
配置
1.C源码
2.子目录的makefile drivers/net/Makefile
3.include/config/auto.config
4.include/linux/autoconf.h


内核子目录
makefile
obj_y+=xxx.o编译进内核
obj_m+=xxx.o编译成模块

.config   make uImage
1..config创建auto.conf
2..config创建autoconf.h

分析Makefile

zImage Image xipImage bootpImage uImage: vmlinux

vmlinux: $(vmlinux-lds) $(vmlinux-init) $(vmlinux-main) $(kallsyms.o) FORCE

vmlinux-init := $(head-y) $(init-y)

head-y		:= arch/arm/kernel/head$(MMUEXT).o arch/arm/kernel/init_task.o

init-y		:= init/
init-y		:= $(patsubst %/, %/built-in.o, $(init-y)) = init/built-in.o


vmlinux-main := $(core-y) $(libs-y) $(drivers-y) $(net-y)


core-y		:= usr/
core-y		+= kernel/ mm/ fs/ ipc/ security/ crypto/ block/
core-y		:= $(patsubst %/, %/built-in.o, $(core-y))
                 = usr/built-in.o kernel/built-in.o mm/built-in.o fs/built-in.o ipc/built-in.o security/built-in.o crypto/built-in.o block/built-in.o

libs-y		:= lib/

libs-y           = lib/lib.a lib/built-in.o

drivers-y	:= drivers/ sound/
                 = drivers/built-in.o sound/built-in.o       
net-y		:= net/

                 = net/built-in.o


vmlinux-all  := $(vmlinux-init) $(vmlinux-main)

vmlinux-lds  := arch/$(ARCH)/kernel/vmlinux.lds


arm-linux-ld -EL  -p --no-undefined -X -o vmlinux
-T arch/arm/kernel/vmlinux.lds 
arch/arm/kernel/head.o arch/arm/kernel/init_task.o  

init/built-in.o 

--start-group  usr/built-in.o  arch/arm/kernel/built-in.o  arch/arm/mm/built-in.o  arch/arm/common/built-in.o  arch/arm/mach-s3c2410/built-in.o  arch/arm/mach-s3c2400/built-in.o  arch/arm/mach-s3c2412/built-in.o  arch/arm/mach-s3c2440/built-in.o  arch/arm/mach-s3c2442/built-in.o  arch/arm/mach-s3c2443/built-in.o  arch/arm/nwfpe/built-in.o  arch/arm/plat-s3c24xx/built-in.o  kernel/built-in.o  mm/built-in.o  fs/built-in.o  ipc/built-in.o  security/built-in.o  crypto/built-in.o  block/built-in.o  arch/arm/lib/lib.a  lib/lib.a  arch/arm/lib/built-in.o  lib/built-in.o  drivers/built-in.o  sound/built-in.o  net/built-in.o --end-group .tmp_kallsyms2.o


第一个文件
连接脚本



10.4
1.处理uboot传入的参数 arch/arm/kernel/head.s
判断是否支持这个CPU
判断是否支持这个单板<--u-boot启动内核时传入的机器ID
建立页表
使能MMU
跳到start_kernel(第一个C函数-->处理启动参数)

挂接：根文件系统
最终目的：执行应用程序


/*
 * Lookup machine architecture in the linker-build list of architectures.
 * Note that we can't use the absolute addresses for the __arch_info
 * lists since we aren't running with the MMU on (and therefore, we are
 * not in the correct address space).  We have to calculate the offset.
 *
 *  r1 = machine architecture number
 * Returns:
 *  r3, r4, r6 corrupted
 *  r5 = mach_info pointer in physical address space
 */
	.type	__lookup_machine_type, %function
__lookup_machine_type:
	adr	r3, 3b                          @ r3 = address of 3b, real address, phy address
	ldmia	r3, {r4, r5, r6}                @ r4 = "." viurtual address of 3b, r5 = __arch_info_begin, r6 = __arch_info_end 
	sub	r3, r3, r4			@ get offset between virt&phys
	add	r5, r5, r3			@ convert virt addresses to
	add	r6, r6, r3			@ physical address space
1:	ldr	r3, [r5, #MACHINFO_TYPE]	@ get machine type
	teq	r3, r1				@ matches loader number?
	beq	2f				@ found
	add	r5, r5, #SIZEOF_MACHINE_DESC	@ next machine_desc
	cmp	r5, r6
	blo	1b
	mov	r5, #0				@ unknown machine
2:	mov	pc, lr

/*
  __arch_info_begin = .;

   *(.arch.info.init)
 
  __arch_info_end = .;

#define MACHINE_START(_type,_name)			\
static const struct machine_desc __mach_desc_##_type	\
 __used							\
 __attribute__((__section__(".arch.info.init"))) = {	\
	.nr		= MACH_TYPE_##_type,		\
	.name		= _name,

#define MACHINE_END				\
};



MACHINE_START(S3C2440, "SMDK2440")
	/* Maintainer: Ben Dooks <ben@fluff.org> */
	.phys_io	= S3C2410_PA_UART,
	.io_pg_offst	= (((u32)S3C24XX_VA_UART) >> 18) & 0xfffc,
	.boot_params	= S3C2410_SDRAM_PA + 0x100,

	.init_irq	= s3c24xx_init_irq,
	.map_io		= smdk2440_map_io,
	.init_machine	= smdk2440_machine_init,
	.timer		= &s3c24xx_timer,
MACHINE_END


static const struct machine_desc __mach_desc_S3C2440	\
 __used							\
 __attribute__((__section__(".arch.info.init"))) = {	\
	.nr		= MACH_TYPE_S3C2440,		\
	.name		= "SMDK2440",
	/* Maintainer: Ben Dooks <ben@fluff.org> */
	.phys_io	= S3C2410_PA_UART,
	.io_pg_offst	= (((u32)S3C24XX_VA_UART) >> 18) & 0xfffc,
	.boot_params	= S3C2410_SDRAM_PA + 0x100,

	.init_irq	= s3c24xx_init_irq,
	.map_io		= smdk2440_map_io,
	.init_machine	= smdk2440_machine_init,
	.timer		= &s3c24xx_timer,
};



struct machine_desc {
	/*
	 * Note! The first four elements are used
	 * by assembler code in head-armv.S
	 */
	unsigned int		nr;		/* architecture number	*/
	unsigned int		phys_io;	/* start of physical io	*/
	unsigned int		io_pg_offst;	/* byte offset for io 
						 * page tabe entry	*/

	const char		*name;		/* architecture name	*/
	unsigned long		boot_params;	/* tagged list		*/

	unsigned int		video_start;	/* start of video RAM	*/
	unsigned int		video_end;	/* end of video RAM	*/

	unsigned int		reserve_lp0 :1;	/* never has lp0	*/
	unsigned int		reserve_lp1 :1;	/* never has lp1	*/
	unsigned int		reserve_lp2 :1;	/* never has lp2	*/
	unsigned int		soft_reboot :1;	/* soft reboot		*/
	void			(*fixup)(struct machine_desc *,
					 struct tag *, char **,
					 struct meminfo *);
	void			(*map_io)(void);/* IO mapping function	*/
	void			(*init_irq)(void);
	struct sys_timer	*timer;		/* system tick timer	*/
	void			(*init_machine)(void);
};





*/


/*
 * This provides a C-API version of the above function.
 */
ENTRY(lookup_machine_type)
	stmfd	sp!, {r4 - r6, lr}
	mov	r1, r0
	bl	__lookup_machine_type
	mov	r0, r5
	ldmfd	sp!, {r4 - r6, pc}


	setup_arch(&command_line);
	setup_command_line(command_line);

static int __init root_dev_setup(char *line)
{
	strlcpy(saved_root_name, line, sizeof(saved_root_name));
	return 1;
}

__setup("root=", root_dev_setup);
#define __setup(str, fn)					\
	__setup_param(str, fn, fn, 0)

#define __setup_param(str, unique_id, fn, early)			\
	static char __setup_str_##unique_id[] __initdata = str;	\
	static struct obs_kernel_param __setup_##unique_id	\
		__attribute_used__				\
		__attribute__((__section__(".init.setup")))	\
		__attribute__((aligned((sizeof(long)))))	\
		= { __setup_str_##unique_id, fn, early }

obsolete_checksetup
do_early_param

parse_early_param



内核启动流程：
arch/arm/kenel/head.S
start_kernel
  setup_arch              //解析u-boot传入的启动参数
  setup_command_line      //解析u-boot传入的启动参数
  parse_early_param
    do_early_param
       从__setup_start到__setup_end，调用early函数
  unknown_bootoption
    obsolete_checksetup
       从__setup_start到__setup_end，调用非early函数
  rest_init
    kernel_init
      prepare_namespace
        mount_root        //挂接根文件系统
      init_post
        //执行应用程序

grep "\"bootloader\"" * -nR

static struct mtd_partition smdk_default_nand_part[] = {
	[0] = {
        .name   = "bootloader",
        .size   = 0x00040000,
		.offset	= 0,
	},
	[1] = {
        .name   = "params",
        .offset = MTDPART_OFS_APPEND,
        .size   = 0x00020000,
	},
	[2] = {
        .name   = "kernel",
        .offset = MTDPART_OFS_APPEND,
        .size   = 0x00200000,
	},
	[3] = {
        .name   = "root",
        .offset = MTDPART_OFS_APPEND,
        .size   = MTDPART_SIZ_FULL,
	}
};


11.1
u-boot:启动内核
内核：启动应用程序（在根文件系统上）
构建：根文件系统

内核怎样启动第一个应用程序：
1.打开/dev/console
	(void) sys_dup(0);
	(void) sys_dup(0);
2.run_init_process


/sbin/init

init程序
1.配置文件
2.解析配置文件
3.执行（用户程序）




busybox->init_main
   parse_inittab
     file = fopen(INITTAB, "r");  //打开配置文件/etc/inittab
        new_init_action
        run_actions(SYSINIT);
	   waitfor(a, 0);
	   delete_init_action(a);
	run_actions(WAIT);
	run_actions(ONCE);
	while (1) {
		run_actions(RESPAWN);
		run_actions(ASKFIRST);
		wpid = wait(NULL);
		while (wpid > 0) {
					a->pid = 0;
		}
	}

new_init_action(ASKFIRST, "-/bin/sh", "/dev/tty2");
static void new_init_action(int action, const char *command, const char *cons)
1.创建一个init_action结构，填充
2.把这个结构放入init_action_list链表

/* Set up a linked list of init_actions, to be read from inittab */
struct init_action {
	struct init_action *next;
	int action;
	pid_t pid;
	char command[INIT_BUFFS_SIZE];
	char terminal[CONSOLE_NAME_SIZE];
};

inittab格式：
# <id>:<runlevels>:<action>:<process>
static void new_init_action(int action, const char *command, const char *cons)
# id = /dev/id, 用作终端：stdin，stdut，stderr，printf，scanf，err
# runlevels:忽略
# action;何时执行

# <action>: Valid actions include: sysinit, respawn, askfirst, wait, once,
#                                  restart, ctrlaltdel, and shutdown.
# process:应用程序或脚本

配置文件：
指定程序
何时执行


从默认的new_init_action反推出默认的配置文件：

::ctrlaltdel:reboot
::shutdown:umount -a -r
::restart:init
tty2::askfirst:-/bin/sh
tty3::askfirst:-/bin/sh
tty4::askfirst:-/bin/sh
::sysinit:/etc/init.d/rcS



/dev/console  /dev/nell
/etc/inittab
配置文件里指定的应用程序
库
init本身，即busybox


最小根文件系统：
1./dev/console   /dev/null
2.init-->busybox
3./etc/inittab
4.配置文件中指定的应用程序
5.C库


make CONFIG_PREFIX=/work/nfs_root/second_fs install


制作最小根文件系统
yaffs2
jffs2


mkyaffs2image second_fs second_fs.yaffs2

mkfs.jffs2 -n -s 2048 -e 128KiB -d second_fs -o second_fs.jffs2

sudo /etc/init.d/nfs-kernel-server restart


PC自己挂载自己
sudo mout -t nfs 192.168.1.19:work/nfs_root/first_fs /mnt
单板挂载
mount -t nfs -o nolock 192.168.1.102:/work/nfs_root/first_fs /mnt


NFS
1.从flash上启动根文件系统，再用命令挂接NFS
2.直接从NFS启动


12.1
VFS：虚拟文件系统



发现每次启动的时候都要先挂载
先
insmod first_drv.ko
再
./firstdrvtest on
./firstdrvtest off
lsmod
rmmod first_drv

之前的设备号讲解很有用，此处可以回头看一下

12.2.3

单片机直接操作物理地址
内核需要用虚拟地址

copy_from_user用户空间到内核空间
copy_to_user内核空间到用户空间

linux中<>括号中的参数不可省略


驱动程序：
驱动程序框架；


主设备号找到驱动程序；
次设备号归驱动程序来用；

12.3

重新开始学习linux
9.1
u-boot环境变量
bootcmd=nand read.jffs2 0x30007FC0 kernel; bootm 0x30007FC0
bootdelay=2
baudrate=115200
ethaddr=08:00:3e:26:0a:5b
netmask=255.255.255.0
mtdids=nand0=nandflash0
mtdparts=mtdparts=nandflash0:256k@0(bootloader),128k(params),2m(kernel),-(root)
ipaddr=192.168.1.10
serverip=192.168.1.100
/linuxrc=console=ttySAC0
bootargs=noinitrd root=/dev/nfs nfsroot=192.168.1.102:/work/nfs_root/tmp/fs_mini_mdev ip=192.168.1.10:192.168.1.102:192.168.1.1:255.255.255.0::eth0:off init=/linuxrc console=ttySAC0
stdin=serial
stdout=serial
stderr=serial
partition=nand0,0
mtddevnum=0
mtddevname=bootloader

Environment size: 583/131068 bytes

set bootdelay 10
save

reset


终极目的启动内核

9.2
01:58
make 100ask24x0_config

100ask24x0_config	:	unconfig
	@$(MKCONFIG) $(@:_config=) arm arm920t 100ask24x0 NULL s3c24x0

