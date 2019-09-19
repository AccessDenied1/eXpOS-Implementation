alias fun R1;
alias pid R2;

if(fun == 2) then
	alias uapn R3;
	uapn = [PROCESS_TABLE + pid*16 +11];
	multipush(R1,R2,R3);
	R1 = 2;
	R2 = uapn;
	call MEMORY_MANAGER;
	multipop(R1,R2,R3);
	return;
endif;
if(fun == 3) then
	multipush(R1,R2,R3);
	R1 = 4;
	R2 = pid;
	call PROCESS_MANAGER;
	R1 = 2;
	R2 = pid;
	call PROCESS_MANAGER;
	multipop(R1,R2,R3);
	[PROCESS_TABLE + pid*16 + 4] = TERMINATED;
	return;
endif;

if(fun == 4) then
	alias page_table R3;
	page_table = [PAGE_TABLE_BASE + pid*20];
	[page_table + 0] = -1;
	[page_table + 1] = "0000";
	[page_table + 2] = -1;
	[page_table + 3] = "0000";
	alias counter R4;
	counter = 0;
	while(counter <= 20) do
		if([PTBR + counter]!=-1) then
			multipush(R4,R5);
			R1=2;
			R2 = [page_table + counter];
			call MEMORY_MANAGER;
			multipop(R4,R5);
			
			[page_table + counter] =-1;
			[page_table + counter+1] ="0000";

		endif;
		counter = counter + 2;
	endwhile;
	return;
endif;
		
