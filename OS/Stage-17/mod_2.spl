alias fun R1;

if(fun == 1) then
	[SYSTEM_STATUS_TABLE + 3] = [SYSTEM_STATUS_TABLE + 3] + 1;
	while([SYSTEM_STATUS_TABLE + 2] ==0) do
		[PROCESS_TABLE + 16*[SYSTEM_STATUS_TABLE + 1] + 4] = WAIT_MEM;
		multipush(R1);
		call SCHEDULER;
		multipop(R1);
	endwhile;
	[SYSTEM_STATUS_TABLE + 3] = [SYSTEM_STATUS_TABLE + 3] - 1;
	[SYSTEM_STATUS_TABLE + 2] = [SYSTEM_STATUS_TABLE + 2] - 1;
	alias c R2;
	c = 0;
	while(c<128) do
		if([MEMORY_FREE_LIST + c] == 0) then
			[MEMORY_FREE_LIST + c] = 1;
			break;
		else
			c = c+1;
		endif;
	endwhile;
	R0 = c;
	return;
endif;
if(fun == 2) then
	alias page_no R2;
	[MEMORY_FREE_LIST + page_no] = [MEMORY_FREE_LIST + page_no] -1;
	if([MEMORY_FREE_LIST + page_no] == 0) then
		[SYSTEM_STATUS_TABLE + 2] = [SYSTEM_STATUS_TABLE + 2] + 1;
	endif;
	alias counter R3;
	counter = 0;
	while(counter < 16) do
		if([PROCESS_TABLE + counter*16 + 4] == WAIT_MEM) then
			[PROCESS_TABLE + counter*16 + 4] = READY;
		endif;
		counter = counter + 1;
	endwhile;
	return;
endif;
	
