# Simple Windows PCAP Build Instructions #

Building gopacket on windows can be a little frustrating, but it does seem to work, once all the right pieces are in all the right places:

  * Install
    * WinPCAP (install BOTH of these):
      * http://www.winpcap.org/install/default.htm (I used 4.1.3)
      * http://www.winpcap.org/devel.htm (I used 4.1.2)
        * Copy WpdPack directory to C:\, creating the directory C:\WpdPack
        * If you put it elsewhere, modify gopacket/pcap/pcap.go to point to your new location
    * MinGW (via tdragon, which sets everything up for you nicely):
      * http://tdm-gcc.tdragon.net/download (I used tdm64)
    * Go:
      * http://golang.org/dl/ (I used go1.3.windows-amd64.msi)
  * Configure
    * Confirm that the following environmental vars are already set up (should be if you installed as described above):
      * PATH (should have C:\Go\bin, C:\Git\cmd, and C:\TDMsomething\bin)
      * GOROOT (should be C:\Go\)
    * Create
      * GOPATH=C:\Users\username\somethingorother\go
  * Run on the command line:
    * go get code.google.com/p/gopacket
    * go install code.google.com/p/gopacket/pcap