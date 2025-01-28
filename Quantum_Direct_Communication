from qiskit.visualization import plot_bloch_multivector, plot_state_qsphere
from qiskit import QuantumCircuit, Aer, execute
import numpy as np


def entangled_pairs(num_pairs):
    
    qc_pairs = QuantumCircuit(num_pairs*2, num_pairs*2)
    
    for i in range(num_pairs):
        qc_pairs.h(i*2)
        qc_pairs.cx(i*2, i*2 + 1)
        
    return qc_pairs


def split_sequences(qc_pairs,shared_paris):
    
   c_sequence = qc_pairs.copy()
   c_sequence.measure(range(0, qc_pairs.num_qubits, 2), range(0, qc_pairs.num_qubits, 2))
   
   job = execute(c_sequence, Aer.get_backend('qasm_simulator'), shots=1)
   output = list(job.result().get_counts())[0]
   
   
   m_sequence = qc_pairs.copy()
   m_sequence.measure(range(1, qc_pairs.num_qubits, 2), range(1, qc_pairs.num_qubits, 2))
   
   return c_sequence, m_sequence, output
    


def encode_message(m_sequence,file,output):
    
    m_sequence.barrier(label='Encode')
    for i in range (len(file)-1):
        out = [output[j] for j in range(0, qc_pairs.num_qubits, 2)]
        
        if (file[i] =='0' and out[i]=='0'):
            pass
        elif(file[i]=='0' and out[i]=='1'):
            m_sequence.z(i*2)
        elif (file[i]=='1' and out[i]=='0'):
            m_sequence.z(i*2)
        elif (file[i]=='1' and out[i]=='1'):
            pass
      
    return m_sequence


def decode_message(m_sequence):
   
    m_sequence.barrier(label= 'Decode')
    for i in range(num_pairs):
        m_sequence.cx(i*2, i*2 + 1)
        m_sequence.h(i*2)
        m_sequence.measure(i*2,i*2)
        
        
    job = execute(m_sequence, Aer.get_backend('qasm_simulator'), shots=1)
    output = list(job.result().get_counts())[0]
    
    decoded_message = ''
    for i in range(num_pairs):
        decoded_message+=output[i*2]
        
    m_sequence.draw('mpl')
        
        
   
    return decoded_message


def FileToBinary(file):
    binary_message = ''.join(format(ord(c), 'b') for c in file)
    msg = ''

    # Encode the message
    for i in range(len(binary_message)):
        msg += binary_message[i]

    return msg

# MAIN
num_pairs = 7
shared_paris = np.random.randint(2, size=num_pairs)

file = input('Enter your message: ')
message = FileToBinary(file)
print(message)

qc_pairs = entangled_pairs(num_pairs)
c_sequence, m_sequence, output = split_sequences(qc_pairs,shared_paris)
print(output)
m_sequence = encode_message(m_sequence, message, output)
decoded_message = decode_message(m_sequence)


print("Encoded Message:")
print(encode_message)
print("Decoded Message:")
print(decoded_message)
