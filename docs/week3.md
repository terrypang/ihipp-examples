
### MPI

## 1.1 V: Communicator in MPI
In the introduction to MPI in the first week we already saw the simple excercise of 'Hello world'. In order to actually write some useful applications, we will need to learn some basic routines. To begin with we need to understand what is a communicator in MPI. As we launch MPI, the entire environment puts all the processes and the cores that are involved in this application and binds them together in what is reffered to as a 'communicator' (image D2P1S12). A communicator  is like a set that binds all the processes together corroborates that only those processses are together in application can communicate with each other. The default communicator that we will use most often is the 
~~~c
MPI_COMM_WORLD
~~~ 
that is already predefined in the header file
~~~c
mpi.h
~~~
### Ranks and Size

Frequently, we use the MPI com world when we will need to use a communicator. Once we actually initialize the MPI environment, all the processes would then be in the communicator. As we can predict it would be nice to be able to distinguish between different processes and this is where the 'ranks' come in. When we initialize the environment, MPI communicator will dedicate a number to each process. This is known as the 'rank'. It is a number that is starting from zero and ends with the size minus one. In this example, as you can see, we have launched the application with seven cores and each of them is given a rank. So this application we have different processes that have been given ranks from zero to six.
This helps us in identifying and using a specific processor. For instance, if we would the processor number two to do perform a certain task, we can command execution of the task, if the rank is '2'.  Later on  when we progress further to learn sending messages between specific processors, we will see how rank is useful. So to say if rank is '2' send a message to rank '6'. This is how we do the sending and receiving of the messages between different processess in MPI.

In order to actually get these numbers, we have two basic routines in MPI. We know now that when we initialize the MPI environment, rank is the number that each of the processor is given and is the number by which you can identify a process. The 'size' is the number that tells us number of processes that are contained within a communicator or simply how many processes are in our application. For instance, when the size is 10, rank goes from zero to nine and so on. 
There are two basic routines for the 'rank'
~~~c
MPI_Comm_rank(MPI_Comm comm, int *rank)
~~~
and for the 'size'
~~~c
MPI_Comm_size(MPI_Comm comm, int *size);
~~~
 (image S13)
 
 ## 1.2.E: Hello World 2.0
 We modify our excercise from the first week.
 
### Goal:
Modify "Hello World" excercise so that
- every process writes its rank and the size of MPI_COMM_WORLD,

- only process ranked 0 in MPI_COMM_WORLD prints "hello world".

## 1.3.D: 
What do you observe when you run the program multiple times?

## 1.4. V: Messages and communications

Until now, we have introduced the MPI and we have used some simple routines such as rank and size, to distinguish between different processes and to actually assign them some numbers that we can recognise and use later. But until now, we haven't done anything useful in a way that we haven't sent any information between the processes. This is where we need to gain an understanding of messages in MPI. For example when we are developing different advanced applications, at one point we will need to exchange information from one process to another.  Usually, this information could be some integer, some values or even arrays etc. This is where messages are used. Messages are packets of data moving between sub-programs. So, as said earlier, if we pack this information to be shared between processses into some message, we can send them over communication network so the other process can receive this message. This is how the data and information is shared bewteen the processes. And of course there is some important information that we will always need to specify in order for the messages to be sent and received efficiently. 
 (image S16)
As we can see in this example, we are trying to send a message from a process at rank '0' to process at rank '2', and in order for this to work, we have to specify some information. 
-Data size and type
First of all, we will need to know, that means that the sender will need to specify what kind of data are we sending. Size of the data inferrs, what is the the 'number'. So for example if we are sending an array of lets say 100 numbers the size would be 100. And as you probably already guessed we would also need to mention what the 'type' of the data is. So is it a character? Is it a double integer? and so on. 
- Pointer to sent or received data
For this data exchange we would need two pointers.These pointers are from the sender that will need to point to its own memory to say, OK, the data I'm trying to send is here. And then the receiver will need to specify the memory where he would like to receive this data. 
-Sending process and receiving process, i.e., the 'ranks'
The MPI environment will need to know who is the sender and who is the receiver. This is where the 'ranks come in'. So in our previous exapmle we will specify that the rank '0' is the sender and the rank '2' is the receiver. 
-Tag of the message
The next information we will need to specify is the 'tag' of the message. A tag is a simple number that we can assign any value from which a receiver can identify the message. For instance, if we would send two messages we can assign one tag as let's say '0' and the other one as tag '1'. This helps the receiver identify and differentiate which message is which. But usually if we will have only one message, we can just put the tag as '0'.
-The communicator, i.e. MPI_COMM_WORLD
The last argument we will need to specify is what is the communicator in which we are sending the messages. In our case here it would be the MPI_COMM_WORLD, but we would eventually learn better about the functions as we will do more excercises and hands on practice. 

### MPI Datatypes
(table S17)
The MPI environment defines its own basic data types. However, if you're familiar with 'C' they're really simple because what you have to do is just put MPI in capital letters before the variable and change everything to capital case. 
So simply put, if you're trying to send an integer, then the type is
~~~
MPI_INT
~~~
and similarly with double, long, character and so on. 
However, as we will get more involved with MPI, we will explore that there is also a way for the user to define its own derived data type. For instance if we're using 'struct' in C, then we  can define that struct as a new MPI data type. This proves to be very useful  because we can just send everything in one message. So this would not require us to send portions of the struct with different messages. But we will dwell deeper into the derived data types in the coming weeks. For today's section we're only using simple data types and that would mostly mean either an int or a double or maybe even a character. 

## 2.1 Types of communication in MPI
There are two criterias by which we divide the types of communications in MPI.
First way to define types of communication is to divide it according to the number of processes that are involved. So, if there are only two processes involved in the communication, meaning only one sender and one receiver then it is reffered to as point to point communication. This is the simplest form of message passing where one process sends a message to another. 
The other type of communication is the collective communication in which we have multiple processes that are involved. This implicates communication between either one processes with many other processes or even many processes communicating with several other processes. So, there are different ways that this can be executed. 
So this is one criteria for distingushing the types of communication i.e  distinction by the number of processes involved.

The second criteria and perhaps more complex is by defining the type of communication into blocking and non blocking types. 
A Blocking routine returns only when the operation has completed. This means that blocking basically implies that if we send a message, we can't proceed to the next steps until the receiver actually returns us information that it has received the message. 
(image S19)
The non blocking communication is more complicated than the simpler blocking counterpart. In this case it returns immediately and allows the  sub-program to perform other work. It differs from the blocking communication in a way that if we send something to the receiver, we can execute some other tasks in between and after some time, we can check if the receiver has actually returned the information that it has receieved the message, or everything is OK. Many real applications, usually employ this type of communication. 

### Point-to-Point Communication

As we already saw in the previous section, Point-to-Point Communication is the simplest communication as it involves only two processes. One of the processes acts as asender and the other one as the receiver. Here, the source or the sender sends a message via the (image S21) communicator to the receiver. In order to do so, the environment has to know who is the sender and who is the receiver and this is taken care of by specifying the 'ranks' of the processes. 
### Sending paradigms
In order to send a message with Point-to-Point Communication , we use the function
~~~c
 MPI_Send(void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm);
 ~~~
 We will now see what arguments this routine actually needs
 
- 'buf' is the pointer to the data with 'count' elements and each of them described with datatype. So first of all, we have to specify the address of the data that we would like to send. So it's a pointer to the data of the center and then the second argument is actually the number of the elements we send. For example if we just send a number one, one integer, for instance, this will be one. If you send an array with 100 integers, this will be one hundred and so on. The third argument this function would like to have is the 'datatype'. So this as we discussed previously, if we are sending an integer, we will have to specify here MPI_int, or  if we're sending a double, this will be a double and so on.
- 'dest' is the rank of the destination process. In this argument we specify the rank of the receiver. So, for instance, in the previous example, this will be '5'.
- 'tag' is an additional integer information transfered with the message. 'tag' is basically a number by which we identify the message. So usually if we just send only one message, we can just put '0' tag there or maybe any number that we would like. 
- The last argument is the communicator and as we already discussed we usually use MPI_Comm world.
So these are the arguments that are the most important information the MPI environment needs in order to know what data is sent and to whom.

## Receiving paradigms
As we saw that sending a message needs a function so quite obviously, the receiver has to call the receiver function. This means that we have two ranks in order that one is the sender, the other one is the receiver. So one will call the MPI_send function and the other will similarly call MPI_Recv to receive. To be able to receive we use the function
~~~c
MPI_Recv(void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Status *status);
~~~
So the arguments needed by this function are similiar to the MPI_send function.
- The 'buf/count/datatype' describe the recevied data.
In the first argument we specify the address of the data, i.e the address where we would like to receive the data and similarly, we put the data type here.
-'source' is the rank of the sender process. We have the a 'number' of the sender, i.e the rank of the sender. In the previous example we would specify number '3', because the rank with number three is trying to send us a message.
- Similar to MPI_send, here we also have a tag. It is really important to make sure that we match this number with the sender. So if the sender specifies that the message has tag '0', the receiver also has to specify the same number here. Otherwise, this will be an infinite loop as we will not receive anything because this message would still not be sent.
- The next argument is the communicator and again we would just use the MPI_Comm world.
- The last argument is not so important for us at the moment as this is something that we will be learning in the next exercise. For now we will just use
~~~c
MPI_STATUS_IGNORE
~~~
We will learn more about this status structure in the coming weeks. 


So let us go through an example to understand again the prerequisites for this communication to work efficiently and how this would actually work in code. (image S24)
Here, the left is the sender and the right is the receiver. Let's suppose that the sender would like to send this buffer array that has 'n' floats over to some other processor. For this it calls this MPI_send routine function. As we already know, the first one is the pointer to the data. So this is 'send buffer'. Then it needs to specify the 'number' of data. In this case, it is 'n'. The second routine is MPI_float, and we need to make sure that this data matches with the one mentioned earlier. As we previously discussed this is the MPI data type that the environment defines, but it has to match with this one, otherwise the communication will not work. Another thing we need to keep in mind (2) is that this data type has to match with the receiver. So we have to be careful when we write these functions that all of these have to match. Now, the receiver has to call the receiver function with the same data type. Here it has to first define an array where it would like to receive this data i.e the receive buffer. So in summary,the sender specifies rank, i.e a number to whom we would like to send the message. The receiver specifies the rank of the source i.e to mention who is the sender? The communicator, of course, has to be the same because they are bound in the same one. But we usually use the MPI_Comm World communicator. The next important part is that the tag i.e the number mentioned of the message has to match. And finally the type i.e the type of the message or type of the the data has to match. 

## 2.2 E: Send and receive

### Goal
Write a basic MPI program which uses MPI_Send and MPI_Recv routines to communicate the number -1 from process 0 to process 1.

### Hint
~~~c
if (rank == 0) { ...
}
else if (rank == 1) {
... }
~~~

## 2.3 E: Ping pong

### Goal 
Write a ping pong program using MPI_Send and MPI_Recv.Two processes ping pong a token back and forth, incrementing it until it reaches a given value.

-Process 0 sends a message to process 1 (ping).
-After receiving this message, process 1 sends a message back to process 0 (pong).
-Each time a message is send, the token is incremented by one. 
-Repeat this ping pong until the value of token reaches 6, i.e. 3 pings and 3 pongs.

## 2.4 D: 
Does the program work for different number of pings and pongs, i.e. 3 pings and 2 pongs?

## 2.4 E: Rotating information around a ring

### Goal
-A set of processes are arranged in a ring.
-Use MPI_Send and MPI_Recv routines to pass an array of numbers around in a ring, starting from rank 0. 
-Program ends when rank 0 process receives the array back and computes its sum.

### Note
Prevent deadlocks, i.e. something is sent and never received or receiver waits forever.

## 2.5 V: Dynamic Receiving with MPI PROBE and MPI STATUS

In the previous excercise when we have implemented the program where we were sending this array along the ring, it was an example of an application in which we already knew that we would be using an array with 100 values. We were already aware of how long it is going to be or in another words how big that message is going to be? How many elements are actually sent? and so on. 
This was important because if we look back into the send and receive routines, we need to specify this 'count'. So, not knowing these numbers already accounts as a problem. In this subsection we will learn that there are two ways to handle this situation, i.e if the size is not known:
-The first way is to send the size of the data as a separate send/recv operation. Therfore, we would send a seperate messages with MPI_send where we can send for e.g the number of elements in an array that we're going to send out later. This works, but sometimes it's not very efficient. 
-The second and a more efficient way is what we eill learn in this subsection with the help of two functions i.e by using MPI_Probe and MPI_Status to obtain the size of sent data.
In order to learn more about these functions and how to use them we need to grasp some concepts that follow.

### Wildcards
Until now, when we have been calling the MPI_sent and MPI_recv functions, we have been specifying some information. In these cases the receiver can use 'wildcards' in some of those arguments. This means in the argument
~~~c
MPI_Recv(void *buf, int count, MPI_Datatype datatype, int source,int tag, MPI_Comm comm, MPI_Status *status);
~~~
that specifies who is the sender, the receiver can just put the source as
~~~c
MPI_ANY_SOURCE
~~~
and this would imply that it doesn't care from where the message will come allowing to receive from any source. For example, if you all would send me a message and I would just like to read one of the messages I would use this. So, I wouldn't care who was sending me the information and I would just read one from one of the sources, so from one of you. 
Similiary we can do this to the tag. In the function where we have to mention the 'number' of the tag we can just use
~~~c
MPI_ANY_TAG
~~~
allowin us to receive a message having any tag. 

### MPI_Status and MPI_Probe









 




 


