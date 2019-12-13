.data
	buffer: .space 34
	secondBuffer: .space 34
	stringMessage: .asciiz "Please enter a new string\n"
	charMessage: .asciiz "Please enter a new char\n"
	intMessage: .asciiz "\nPlease enter a new number\n"
	invalidArgumentMessage: .asciiz "\nInvalid argument (number should be between 1-9)\n"
	count_abc_Result: .asciiz "\nLongest consecutive small letters count = "
	count_char_Result: .asciiz "\nOccurances of the char in the string = "
	deleteMessage:.asciiz "\nDeleted each char in the index of the string that is multiple of the supplied number\n"
	reverseMessage:.asciiz "\nNew string in reverse ->"
.text	
.globl main	
main:
	# print int message
	la $a0,intMessage
	jal printString
	# read number from user
	li $v0,5
	syscall
	move $a0,$v0
	jal fibonacci
	move $a0,$v0
	jal printINT
	after:
        j exit


# this functions takes a character (symbol) and a number (n) as argument and prints the symbol n*n times
print_symbol:
	# pre
	addi $sp, $sp, -12
	sw $ra, 0($sp)
	sw $s0, 4($sp)
	sw $s1, 8($sp)
	loopOutter: beq $t4,$s0 print_symbol_end
		li $a0,'\n'
		jal printString
		add $t4,$t4,1 # $t4++
		and $t3, $t3, 0 # assign 0 to t3
		loopInner: beq $t3,$s0 loopOutter
		add $t3,$t3,1
		# print command $t1 char
		move $a0,$s1 
		li $v0, 11 #syscall 11 prints the lower 8 bits of $a0 as an ascii character.
		syscall
		j loopInner
	print_symbol_end:
		sw $ra, 0($sp)
		sw $s0, 4($sp)
		sw $s1, 8($sp)
		addi $sp, $sp, 12
		# return
		jr $ra
		
count_abc:
	### Description ###
	# this function takes a string (s) and a character (c) and return the longest consecutive alphabet letters in ascending order
	# Params -> 
	#	maxStreak - the length of the longest consecutive small letters 
	#	currentStreak - current length of the consecutive small letters
	#	curChar - current character in the loop
	#	lastValidChar - last letter (starts as empty char)
	# We loop througth the string, 
	# if curChar is a big letter - assign 0 to currentStreak and lastValidChar
	# if curChar is a small letter - check if lastValidChar is empty or current letter is bigger than lastValidChar
	# if yes - increment currentStreak by 1 and assign the new value to maxStreak if its bigger
	# else - assign 1 to currentStreak (start counting)
    # assign lastValidChar to the current letter
    # return the maxStreak
	# Python representation of the algorithm
	# def func(s):
	#     maxStreak = 0
	#     currentStreak = 0
	#     lastValidChar = ''
	#     for c in s:
	#         if c < 'a' or c > 'z':
	#             currentStreak = 0
	#             lastValidChar = ''
	#         else:
	#             if lastValidChar == '' or c > lastValidChar:
	#                 currentStreak += 1
	#                 if currentStreak > maxStreak:
	#                     maxStreak = currentStreak
	#             else:
	#                 currentStreak = 1
	#         lastValidChar = c
	#     return maxStreak
	#	
	# pre
	addi $sp, $sp, -16
	sw $ra, 0($sp)
	sw $s0, 4($sp)
	sw $s1, 8($sp)
	sw $s2, 12($sp)

	and $t0,0 # curChar
	and $t1,0 # is Small letter
	and $t2,0 # maxStreak
	and $t3,0 # currentStreak
	and $t4,0 # lastValidChar	
	move $s0,$a0 # s0 hold the string memory 
	# body	
	count_abc_loop: 
		lb $t0, 0($s0)	# load the current char in the loop
		beq $t0,$zero,count_abc_postLoop	# go to postLoop if we reach null character (end of string)
		move $a0,$t0	# move the char to $a0
		jal checkIfSmallLetter # check if the cur letter is a small letter
		move $t1,$v0
		beq $t1, 1, count_abc_isSmallLetter
		count_abc_isBigLetter:
		and $t3,0 # curStreak = 0
		and $t4,0 # lastvalidChar = ''
		j count_abc_continue
		count_abc_isSmallLetter:
			#   if lastValidChar == '' or curChar > lastValidChar:
			beqz $t4, count_abc_ifValid
			bgt $t0,$t4, count_abc_ifValid
			and $t3,0
			addi $t3,$t3,1
			j count_abc_continue
			count_abc_ifValid:
				addi $t3,$t3,1
				and $t2,0
				addi $t2,$t3,0
		count_abc_continue:
			lb $t4, 0($s0) # store the last char in $t4
			addi $s0, $s0, 1 # increment string pointer
		j count_abc_loop
	count_abc_postLoop:
		move $a0,$t2
		# post	
		move $v0,$t2
		
		lw $ra,0($sp)
		lw $s0,4($sp)
		lw $t1,8($sp)
		lw $t2,12($sp)
		addi $sp,$sp,16
		# return
		jr $ra
		
		
checkIfSmallLetter:
		# Description #
		# return 1 if char in argument is small letter -  else return 0
	        # pre - only using temp registers no need to store anything
	        # rest registers
	        move $t0, $a0 # hold char argument
	        and $t1,0
	        or $t1,1	# isSmallLetter = true
	        # if char >= 'a' && char <= 'z'
        	blt $t0,97,checkIfSmallLetter_not # t0 < a
	        bgt $t0,122,checkIfSmallLetter_not # t0 > z
	        j checkIfSmallLetter_postCheck
		checkIfSmallLetter_not:
		and $t1,0 # isSmallLetter = false
		# post	
		checkIfSmallLetter_postCheck:
		move $v0,$t1 # return isSmallLetter
		# return
		jr $ra

count_char:
	# Description #
	# this function takes a string (s) and a char (c) and return the number of occurances of c in s
	# We loop througth the string, 
	# if curChar is equal to the supplied char then increment counter by 1
	# after finish loop return the counter
	# Python representation of the algorithm
	# def count_char(str,c):
	#	counter = 0
	#	for  curChar in str:
	#		if curChar == c:
	#			counter+=1
	#	return counter
	
	# pre
	addi $sp, $sp, -20
	sw $ra, 0($sp)
	sw $s0, 4($sp)
	sw $s1, 8($sp)
	sw $s2, 12($sp)
	sw $s3, 16($sp)
	
	# reset registers
	and $s0,0 # string memory
	and $s1,0 # supplied char
	and $s2,0 # counter = 0
	and $s3,0 # curChar = 0
	
	# body
	move $s0,$a0 # argument 1 - string memory adress
	move $s1,$a1 # argument 2-  supplied char 
	count_char_loop: 
		lb $s3, 0($s0) # load the current char in the loop
		beq $s3,$zero,count_char_postLoop # go to postLoop if we reach null character (end of string)
		bne $s3,$s1 skip
		addi $s2,$s2,1
		skip: 
		addi $s0, $s0, 1 #load increment string pointer
		j count_char_loop
	count_char_postLoop:
		move $a0,$s2
	# post
	move $v0,$s2
	lw $ra, 0($sp)
	lw $s0, 4($sp)
	lw $s1, 8($sp)
	lw $s2, 12($sp)
	lw $s3, 16($sp)
	addi $sp,$sp,20
	# return
	jr $ra
	
delete:	
	# Description #
	# this functions takes a string (s) and a number (n) and removes all chars in the multiple of n indexes (ie n,2n,3n...)
	# pre
	addi $sp, $sp, -20
	sw $ra, 0($sp)
	sw $s0, 4($sp)
	sw $s1, 8($sp)	
	sw $s2, 12($sp)
	sw $s3, 16($sp)	
	and $s0,0
	and $s1,0
	and $s2,0
	and $s3,0
	and $t0,0
	and $t1,0
	
	# body
	move $s0,$a0 # s0 hold the string memory 
	move $s1,$a1 # first number (x)
	addi $s2,$s2,1 # counter index
	la $s3,secondBuffer # load new string address
	# body
	# loop through the string and create new one without the unwanted chars indexes
	delete_loop: 
		lb $t0, 0($s0)	# load the current char in the loop
		beq $t0,$zero,delete_postLoop	# go to postLoop if we reach null character (end of string)
		rem $t1,$s2,$s1 # check if index % x equals to 0 (means we are at multiple of x)
		beqz  $t1,delete_loop_continue
		# add current char to new string
		sb $t0,0($s3)
		addi $s3,$s3,1
		delete_loop_continue:
			add $s2,$s2,1 #increment counter
			add $s0, $s0, 1 # increment string pointer
		j delete_loop
	delete_postLoop:
	la $s0,buffer # load new string address
	la $s3,secondBuffer # load new string address
	delete_loop2: 
		lb $t0, 0($s0)	# load the current char in the loop from original string
		lb $t1, 0($s3)	# load the current char in the loop from new string
		beq $t0,$zero,delete_postLoop2	# go to postLoop if we reach null character (end of string)
		sb $t1,0($s0)
		sb $zero,0($s3)
		add $s0, $s0, 1 # increment string pointer
		addi $s3,$s3,1 # increment new string pointer
		j delete_loop2
	delete_postLoop2:
	# post	
	lw $ra,0($sp)
	lw $s0,4($sp)
	lw $s1,8($sp)
	lw $s2,12($sp)	
	lw $s3,16($sp)		
	addi $sp,$sp,20

	# return
	jr $ra
reverse:
	# Description #
	# this function recieves a string (s) and prints it in reverse order
	# loop through the string while increasing count by 1, when we reach null char (end of stirng) loop backwar until-
	#	count reaches zero while printing the currnet string char (pointed at by the string pointer)
	# pre
	addi $sp, $sp, -12
	sw $ra,0($sp)
	sw $s0,4($sp)
	sw $s1,8($sp)
	move $s0,$a0 # s0 hold the string memory 
	and $s1,0
	reverse_countChars_loop: 
		lb $t0, 0($s0)	# load the current char in the loop from original string
		beq $t0,$zero,reverse_countChars_postLoop	# go to postLoop if we reach null character (end of string)
		add $s0, $s0, 1 # increment string pointer
		addi $s1,$s1,1 # increment counter by 1
		j reverse_countChars_loop
	reverse_countChars_postLoop:
	reverse_print_loop: 
		lb $t0, 0($s0)	# load the current char in the loop from original string
		bltz  $s1,reverse_print_postLoop	# go to postLoop if we reach null character (end of string)
		move $a0,$t0
		jal printChar
		add $s0, $s0, -1 # increment string pointer
		addi $s1,$s1,-1 # increment counter by 1
		j reverse_print_loop
	reverse_print_postLoop:
		lw $ra,0($sp)
		lw $s0,4($sp)
		lw $s1,8($sp)
		addi $sp,$sp,12
		# return
		jr $ra

fibonacci:
### 	Description 	###
# this function takes a number as an argument and return the result of fibonacci sequence in the n-th place
addi $sp, $sp, -12
sw $ra, 8($sp)
sw $s0, 4($sp)
sw $s1, 0($sp)
move $s0, $a0
li $v0, 1 
ble $s0, 2, fibonacciExit
addi $a0, $s0, -1 # f(n-1)
jal fibonacci
move $s1, $v0 # store result of f(n-1) to s1
addi $a0, $s0, -2 # f(n-2)
jal fibonacci
add $v0, $s1, $v0 # add result of f(n-1)
fibonacciExit:
lw $ra, 8($sp)
lw $s0, 4($sp)
lw $s1, 0($sp)
addi $sp, $sp, 12
# Return
jr $ra
	
printString:
	li $v0, 4       # print string
	syscall
	# return
	jr $ra
	
printChar:
	li $v0, 11       # print char
	syscall
	# return
	jr $ra

printINT:	
	li $v0, 1       # print int
	syscall
	# return
	jr $ra

exit:
        li $v0, 10      # end program
        syscall
