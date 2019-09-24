alias userSP R1;
userSP = SP;
alias currentPid R10;
currentPid = [SYSTEM_STATUS_TABLE + 1];

[PROCESS_TABLE + 16 * currentPid + 13] = SP;             // Save current SP into UPTR
SP = [PROCESS_TABLE + 16 * currentPid + 11] * 512 - 1;   // set SP to start of Uarea page      


[PROCESS_TABLE + 16 * currentPid + 9] = 9;

//get the filename

alias file R0;
file = [[PTBR + 2*((userSP-4)/512)]*512 + ((userSP - 4)%512)];

alias counter R2;
counter = 0;
while(counter < MAX_FILE_NUM) do
	if([INODE_TABLE	+ counter*16 + 1] == file && [INODE_TABLE + counter*16 ] == EXEC) then
		break;
	else
		counter = counter+1;
endif;
endwhile;
if(counter == MAX_FILE_NUM) then 
	[[PTBR + 2*((userSP - 1)/512)]*512 + ((userSP - 1)%512)] = -1;
	SP = [PROCESS_TABLE + 16 * currentPid + 13];
	[PROCESS_TABLE + 16 * currentPid + 9] =0;
	ireturn;
endif;

alias inode_index R4;
inode_index = counter;
multipush(R0,R1,R2,R3);
R1 = 3;
R2 = [SYSTEM_STATUS_TABLE + 1];
call PROCESS_MANAGER;
multipop(R0,R1,R2,R3);
alias uapn R3;
uapn = [PROCESS_TABLE + 16*currentPid + 11];

[MEMORY_FREE_LIST + uapn] = [MEMORY_FREE_LIST + uapn] + 1;

[SYSTEM_STATUS_TABLE + 2] = [SYSTEM_STATUS_TABLE + 2] -1;

SP = uapn*512 -1;

[PROCESS_TABLE + 16 * currentPid + 4] = RUNNING;
[PROCESS_TABLE + 16 * currentPid + 7] = inode_index;

alias page_table R11;
page_table = PAGE_TABLE_BASE + currentPid*20;

[page_table + 0] = 63;
[page_table + 1] = "0100";
[page_table + 2] = 64;
[page_table + 3] = "0100";

multipush(R0,R1,R2,R3);
R1 = 1;
call MEMORY_MANAGER;
[page_table + 4]=R0;
[page_table + 5]="0110";

R1 = 1;
call MEMORY_MANAGER;
[page_table + 6]=R0;
[page_table + 7]="0110";

R1 = 1;
call MEMORY_MANAGER;
[page_table + 16]=R0;
[page_table + 17]="0110";

R1 = 1;
call MEMORY_MANAGER;
[page_table + 18]=R0;
[page_table + 19]="0110";

multipop(R0,R1,R2,R3);

alias no_block R5;

no_block = [INODE_TABLE + inode_index*16 +2]/512;
if([INODE_TABLE + inode_index*16 +2]%512 !=0) then
	no_block =no_block+1;
endif;

counter = 8;
while(no_block >0) do
	multipush(R0,R1,R2,R3);
	R1 = 1;
	call MEMORY_MANAGER;
	multipop(R0,R1,R2,R3);
	[page_table + counter]=R0;
	counter = counter +1;
	[page_table + counter]="0100";
	counter = counter +1;
	no_block = no_block -1;
endwhile;

counter = 0;
while(counter < 4) do

    if([INODE_TABLE + inode_index * 16 + (counter + 8)] != -1) then
        loadi([page_table + 8 + 2 * counter], [INODE_TABLE + inode_index * 16 + 8]);
    endif;
    counter = counter + 1;

endwhile;


[[page_table+16]*512] = [[page_table+8]*512 + 1];

SP = 8*512;
[PROCESS_TABLE + 16 * currentPid + 9] = 0;

ireturn;





