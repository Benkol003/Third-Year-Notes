### Process for line drawing modules

req goes high
ack set high 
req goes low

busy goes high

line drawing phase:

de_req set high
de_ack goes low
de_req set low
 data synchronised

#------------ 

convert pixel addresses to framestore addresses:

framestore is 640 bytes / 160 words (4-byte) wide
pixel is 1-byte color


(x,y) -> de_data = 640(y//4) + x (integer division)

de_nbyte = y % 4

also need to perform linear interpolation between the pixels

(10,10) to (15,20)

0x642 to 0xc83 from questa

framestore appears to be rotated? x increases by 160
