# Gauss-Jordan
def gaussianElimination(matrix, vector)
0.upto(matrix.length - 2) do |pivotIdx|

    maxRelVal = 0
    maxIdx = pivotIdx
    (pivotIdx).upto(matrix.length - 1) do |row|
    relVal = matrix[row][pivotIdx] / matrix[row].map{ |x| x.abs }.max
    if relVal >= maxRelVal
        maxRelVal = relVal
        maxIdx = row
    end
    end

   
    matrix[pivotIdx], matrix[maxIdx] = matrix[maxIdx], matrix[pivotIdx]
    vector[pivotIdx], vector[maxIdx] = vector[maxIdx], vector[pivotIdx]

    pivot = matrix[pivotIdx][pivotIdx]
    
    (pivotIdx+1).upto(matrix.length - 1) do |row|
    
    factor = matrix[row][pivotIdx]/pivot

    matrix[row][pivotIdx] = 0.0
    
    (pivotIdx+1).upto(matrix[row].length - 1) do |col|
        matrix[row][col] -= factor*matrix[pivotIdx][col]
    end
    vector[row] -= factor*vector[pivotIdx]
    end
end

return [matrix,vector]
end


def backSubstitution(matrix, vector)
(matrix.length - 1).downto( 0 ) do |row|
    tail = vector[row]
    (row+1).upto(matrix.length - 1) do |col|
    tail -= matrix[row][col] * vector[col]
    matrix[row][col] = 0.0
    end
    vector[row] = tail / matrix[row][row]
    matrix[row][row] = 1.0
end
end


require 'pp'


matrix =
[
    [1.0, 1.0, 1.0, 1.0],
    [0.0, 1.0, 2.0, 3.0],
    [1.0, 2.0, 4.0, 8.0],
    [0.0, 1.0, 4.0, 12.0],
]
vector = [1.0, 0.0, 2.0, 0.0]


matrix_backup = Marshal.load(Marshal.dump(matrix))
vector_backup= vector.dup


gaussianElimination(matrix, vector)

backSubstitution(matrix, vector)


pp matrix
pp vector


pass = true

0.upto(matrix_backup.length - 1) do  |eqn|
sum = 0
0.upto(matrix_backup[eqn].length - 1) do |term|
    sum += matrix_backup[eqn][term] * vector[term]
end
if (sum - vector_backup[eqn]).abs > 0.0000000001
    pass = false
    break
end
end

if pass
puts "Verification PASSED."
else
puts "Verification FAILED."
end
