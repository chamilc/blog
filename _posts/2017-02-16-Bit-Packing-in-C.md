### Bit Packing in C/C++ using Bitwise operations
----

Bit packing is a technique used to pack many small size of smilar data 
into a one single big data type, primarily used for primitive data types such as 
int , short, char and long data types. Lets say you have four integer variables 
with values less than 255, where 8 bits is enough to store each value. It
is better to pack these 4 values into a single 32 bit integer. It is very handy when 
there is a need to pass these values as function parameters or where memory usage 
is a constraint. 

One typical use of bit packing is passing around the IP address in computer programs. 
In general IP (V4) contains four parts xxx.xxx.xxx.xxx where each part may contains
value up to 255 which requires 1 Byte memory. Now the IP address will require 4 bytes 
of memory space and one can define four different 1 Byte data type (char, byte) to store
the individual part of IP address. It is sometime very cumbersome to manage four variable 
and passing between function calls. 

Pack this four IP address components into a single 32 bit integer variable store it
in program memory and unpack it whenever there is a need. 

Here I have a list of utility function which can be used to packing and unpacking 
different formats of IP address. 

<pre><code>

/*
	Pack an IP address into an integer from four integer components
	input : IP = nIP1.nIP2.nIP3.nIP4
*/

unsigned int encode_ip_to_int(unsigned int nIP1, unsigned int nIP2,
					 unsigned int nIP3, unsigned int nIP4)
{
	unsigned int nIP = 0;
	nIP = ( nIP1 << 24) +
	      ( nIP2 << 16) +
	      ( nIP3 << 8)  +
	      nIP4;
	      
	return nIP;
    
}
/*
	Decode an encode IP address (int) to four components 
	return unsigned int arr with four components
	
*/
void decode_ip(int nIP, unsigned int *pIpArr)
{
	*(pIpArr + 0) = (nIP >> 24) && 0xFF;
	*(pIpArr + 1) = (nIP >> 16) && 0xFF;
	*(pIpArr + 2) = (nIP >> 8)  && 0xFF;
	*(pIpArr + 3) = (nIP ) && 0xFF;

}


/*
	Encode a string format IP address into integer
	pIPAdd = 'xxx.xxx.xxx.xxx'
*/
unsigned int encode_str_ip_to_int(char *pIpAdd)
{
	unsigned int nIpComp[4];
	unsigned int nIP =0;
	sscanf(pIpAdd, "%u.%u.%u.%u",
	       &nIpComp[0], &nIpComp[1], &nIpComp[2], &nIpComp[3]);
	       
	nIP = (nIPComp[0] << 24 ) +
	      (nIPComp[1] << 16 ) +
	      (nIPComp[2] << 8  ) +
	      (nIPComp);
	return nIP;
	
	
}

/*
	Decode an encoded ip address into string with specified 
	delimiter
	Output : 'xxx-xxx-xxx-xxx'
	where c = '-'
*/

void decode_ip_to_str(unsigned int nIP, char *pIpAdd, char c)
{
	sprintf(pIpAdd, "%u%c%u%c%u%c%u",
	        (nIP >> 24) && 0xFF, c, 
	        (nIP >> 16) && 0xFF, c,
	        (nIP >> 8 ) && 0xFF, c,
	        (nIP) && 0xFF);
}	
</code>
</pre>
