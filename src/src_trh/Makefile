# compiler flags
CC=gcc
LIBS=-lm 
#INCLUDE=-I. 
# gcc  -g -O2   -o RNAhybrid  rnahybrid.o hybrid_core.o energy.o input.o fasta.o plot.o  -lgd -lm
#CFLAGS= -O2 -m32 -static    
CFLAGS= -O2 -static    

trh: rnahybrid.o hybrid_core.o energy.o input.o fasta.o 
	$(CC) $(CFLAGS) $(INCLUDE) -o trh rnahybrid.o energy.o hybrid_core.o input.o fasta.o $(LIBS)

input.o: input.c
	$(CC) $(CFLAGS) $(INCLUDE) -c input.c

fasta.o: fasta.c
	$(CC) $(CFLAGS) $(INCLUDE) -c fasta.c

hybrid_core.o: hybrid_core.c
	$(CC) $(CFLAGS) $(INCLUDE) -c hybrid_core.c

energy.o: energy.c
	$(CC) $(CFLAGS) $(INCLUDE) -c energy.c

rnahybrid.o: rnahybrid.c
	$(CC) $(CFLAGS) $(INCLUDE) -c rnahybrid.c

clean:
	rm -f *.o core.* 
