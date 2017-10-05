# assignment-2017-4
algorithmoi

import sys

def Num(array):
	#print(array)
	if isinstance( array[0] , int ):
		test = ''
		for i in range(len(array)):
			test += str(array[i])
	else:
		test = ''.join(array)
	#print (test)
	result = int(test, 2)
	#print("Num = " + str(result))
	return result
	
def dimensions(X):
	print("rows: " + str(len(X)))
	print("columns: " + str(len(X[0])))
	print("3d dimension: " + str(len(X[0][0])))
	
def createArray(rows, columns):
	result = [[0 for x in range(columns)] for y in range(rows)]
	return result
	
def addArrays(X,Y):
	result = createArray( len(X), len(X[0]) )
	if len(X) == len(Y) and len(X[0]) == len(Y[0]):
		for row in range (len(X)):
			for col in range(len(X[0])):
				result[row][col] = X[row][col] + Y[row][col]
				if result[row][col] > 1:
					result[row][col] = 1
	#print(result)
	return result
	
def addRow(X,Y):
	result = []
	maxNumber = max(len(X),len(Y))
	#print("Add rows:")
	print("lenX: " + str(len(X)) + ", lenY: " + str(len(Y)))
	#print("X:")
	#print(X)
	#print("Y:")
	#print(Y)
	for i in range(maxNumber):
		if i <= len(X) and i <= len(Y):
			temp = X[i]+Y[i]
			if temp > 1:
				temp = 1
			result.append(temp)
	#print("Result:")
	#print(result)
	return result
	
def SplitArrayByColumn(X,numberPartitions):
	rows = len(X)
	columns = len(X[0])
	partColumns = int( ceil(columns/float(numberPartitions)) )
	result = []
	for i in range(numberPartitions):
		temp = createArray(rows,partColumns)
		result.append(temp)
	for i in range(numberPartitions):
		for row in range (rows):
			for col in range(partColumns):
				if columns > col + (i*partColumns):
					result[i][row][col] = X[row][col+(i*partColumns)]
	#print(result)
	return result
	
def SplitArrayByRow(X,numberPartitions):
	rows = len(X)
	columns = len(X[0])
	partRows = int( ceil(rows/float(numberPartitions)) )
	result = []
	for i in range(numberPartitions):
		temp = createArray(partRows,columns)
		result.append(temp)
	for i in range(numberPartitions):
		for row in range (partRows):
			for col in range(columns):
				if rows > row + (i*partRows):
					result[i][row][col] = X[row+(i*partRows)][col]
	#print(result)
	return result
	
def print2DimensionArray(X):
	print("[")
	for i in range(len(X)):
		print(X[i])
	print("]")
	
def print3DimensionArray(X):
	print("[")
	for i in range(len(X)):
		print2DimensionArray(X[i])
	print("]")
	
def FourRussians(A,B,n):
	partitionsForArrays = int(ceil(log(n,2)))
	Ai = SplitArrayByColumn(A, partitionsForArrays)
	Bi = SplitArrayByRow(A, partitionsForArrays)
	print("------Ais-----")
	print3DimensionArray(Ai)
	print("------Bis-----")
	print3DimensionArray(Bi)
	m = floor( log(n,2) ) 
	print("m= " + str(m))
	print("max j = " + str(int(pow(2,m))))
	C = createArray(n, n) #Create array and initialize it to zero
	for i in range(1,int(ceil(n/float(m))+1)):
		rs = createArray(int(pow(2,m)), n)
		bp = 1
		k = 0
		for j in range(1, int(pow(2,m))):
			rs[j] = addRow( rs[j - int(pow(2,k))] , Bi[i][-(k+1)] ) #?
			if bp == 1:
				bp = j + 1
				k = k + 1
			else:
				bp = bp - 1
		Ci = createArray(n,n)
		print2DimensionArray(Ai[i])
		print2DimensionArray(rs)
		for j in range(n):
			#print Num(Ai[i][j])
			Ci[j] = rs[Num(Ai[i][j])] 
		C = addArrays(C,Ci)
		return C

def readFile(filename):
	test = []
	file = open(filename,"r")
	for line in file:
	    number_strings = line.rstrip("\r\n").split(",") # Split the line on runs of whitespace
	    numbers = [int(n) for n in number_strings] # Convert to integers
	    test.append(numbers) 
	file.close()
	return test
	
def writeFile(filename, array):
	file = open(filename, "w")
	for row in range(len(array)):
		for col in range(len(array[0])-1):
			file.write(str(array[row][col]) + ",")
		file.write(str(array[row][len(array[0])-1]))
		if row < len(array)-1:
			file.write("\r\n")
	file.close()

#A = readFile("array1.txt")
#B = readFile("array2.txt")
A = B = [[-1]]
if len(sys.argv)>=3:
  A = readFile(sys.argv[1])
  B = readFile(sys.argv[2])
elif len(sys.argv)==1:
  print "Not enough parameters"

print("------A-----")
print2DimensionArray(A)
print("------B-----")
print2DimensionArray(B)

result = [[-1]]
#result = FourRussians(A,B,13)
print("------result-----")
print2DimensionArray(result)

writeFile("result.txt", result)

def floor(num):
  return int(num)

def ceil(num):
  roundedDown = floor(num)
  if num==roundedDown:
    return num
  else:
    return roundedDown+1
  
def pow(base, exp):
  return base ** exp

def ln(x):
  n = 1000000.0
  return n * ((x ** (1/n)) - 1)
  
def log(x, base):
  return ln(x)/ln(base)
  
