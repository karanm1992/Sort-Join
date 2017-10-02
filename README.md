# Sort-Join
Code Base for a index-based simple sort join
The algorithm used to join these two given tuples will be a index-based simple sort join using
a sorted index. There are a few variations of the simple sort join that need to be applied here since the
relation S and the index are sorted on Y. This algorithm is a two-pass algorithm which is required
because of the given condition that neither the blocks of R or S can all completely fit into memory. In
this algorithm,

-We first load the sorted secondary index of R whose size in much smaller than S and can thus
fit into the memory without taking up significant space.
-We then load the number of blocks of S from disk that can fit into M-1 blocks of the remaining
space in the memory.
-Once the blocks have been loaded, we loop through the Y attribute in the R index, comparing
them to the Y attribute of the tuples of S.
-If a match is found, the corresponding block required to extract the tuple of R is brought in from
disk using the pointer of that tuple in the index. This block is stored in the single empty block in
the memory. At this point, the memory is completely utilized.
-Once this block has been brought in, the appropriate and required tuple of R is extracted from
this block, we join the remaining tuples of R and S to form a joined tuple.
This joined tuple is fed to the output buffer. If the output buffer is full, it is emptied out to the
disk.
-Once all the tuples in memory of S are done being checked, the next required tuples of S are
brought in till the time all are done.
-The output buffer is now flushed to output any joined tuples which may have remained in the
buffer.
-The final output containing all the tuples is now present in the disk.

Disk storage has been simulated by using the disk storage of the running machine. Text files,
containing the required tuples of R and S are placed at the location of the code. These files are read and
stored in an in-memory class, each object of which represents a tuple in the corresponding relation.
There are also classes which represent the blocks of these tuples. They simply contain arrays of the
above mentioned tuples to simulate blocked loading of tuples. I have assumed 3 tuples per block for
both R and S.
Memory has been simulated by a java class. It has a single instance which simulates the
required memory structure. The object of this memory structure is capable of holding the index
(represented by a java util Map), one block of R and 2 blocks of S. This size restriction ensures the
given memory condition of B(R) >M and B(S) >M since all block of R or S will not fit into memory.
Index has been simulated as a java util Map which is built and is present in the disk simulation,
at the same level where the relations R and S lie after their respective files have been parsed. This index
is later brought into memory before the tuple matching begins.

The code can be run by ensuring the following four files are in the same directory - 
	1. NaturalJoin.java
	2. rTuple.java
	3. rBlock.java
	4. sTuple.java
	5. sBlock.java
	6. Memory.java
	7. JoinTuple.java

Run the following commands on a linux terminal or dos terminal - 

javac NaturalJoin.java
java NaturalJoin

If the code is run through eclipse, the the input files need to be in the projet directory.
If the code is run through the terminal or command prompt, then the input files need to be present in the same directory as the java files.
Final output is present in a file called output.txt which will be generated in the same directory.
