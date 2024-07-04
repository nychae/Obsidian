- Function
	1)  01 (0x01) Read Coils
		- 원격 장치에서 코일의 연속 상태를 1 ~ 2000까지 판독하는데 사용
		- 요청 
			- 시작 주소, 즉 첫 번째 코일의 주소 및 코일 수를 지정(0부터 시작)
		- 응답 
			- 데이터 필드의 비트 당 하나의 코일
			- 상태
				- 1: ON
				- 0: OFF
			
	2)  02 (0x02) Read Discrete Inputs
		- 원격 장치에 있는 불연속적인 입력(discrete inputs)의 연속된 상태를 1에서 2000까지 읽는데 사용
		- 
	1)  03 (0x03) Read Holding Registers
	
	4)  04 (0x04) Read Input Registers
	
	5)  05 (0x05) Write Single Coil
	
	6)  06 (0x06) Write Single Registers
	
	7)  15 (0x0F) Write Multiple Coils
	
	8)  16 (0x10) Write Multiple Registers