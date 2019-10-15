import socket
import sys

def forward_message(number_of_ports,base_value_of_ports,buffer_size):
    #use the global sock object already created
    global sock
    lowest_usn = 54
    ports_array = []
    for i in range(number_of_ports):
        ports_array.append(40000 + 100 * i + lowest_usn)
    print(ports_array)

    for port in ports_array:
        # this is localhost now but can be made dynamic to receive packets from the input port to forward it to the output ports
        server_address = ('localhost',port)
        message = "hello from the other side " + ":"  + str(port)
        
        #send the message to the above declared port and server
        sent = sock.sendto(message.encode(),server_address) 


#should be +1 than the number of inputs given actually,cause the first argument is always the name of the program
if(len(sys.argv) != 5):
    print("Usage: python3 router.py <numberOfPorts> <baseValueOfPorts> <bufferSize in bytes> <transmitRate>\n")
    exit(1)

#name_of_program is dummy!!
name_of_program,number_of_ports,base_value_of_ports,buffer_size,transmit_rate = sys.argv

# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

#set buffer size to buffer size from the arguments
sock.setsockopt(socket.SOL_SOCKET,socket.SO_SNDBUF,int(buffer_size))
sock.setsockopt(socket.SOL_SOCKET,socket.SO_RCVBUF,int(buffer_size)) 

#invoke the function which sends out the udp packets
forward_message(int(number_of_ports),int(base_value_of_ports),int(buffer_size))
