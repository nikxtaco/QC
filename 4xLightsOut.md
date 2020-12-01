# 4xLightsOut



## Unfinished code...
```
from qiskit import QuantumCircuit, ClassicalRegister, QuantumRegister

# Calculate what the light state will be after pressing each solution candidate. 
def flip_tile(qc,flip,tile):
    qc.cx(flip[0], tile[0])
    qc.cx(flip[0], tile[1])
    qc.cx(flip[0], tile[3])
    qc.cx(flip[1], tile[0])
    qc.cx(flip[1], tile[1])
    qc.cx(flip[1], tile[2])
    qc.cx(flip[1], tile[4])
    qc.cx(flip[2], tile[1])
    qc.cx(flip[2], tile[2])
    qc.cx(flip[2], tile[5])
    qc.cx(flip[3], tile[0])
    qc.cx(flip[3], tile[3])
    qc.cx(flip[3], tile[4])
    qc.cx(flip[3], tile[6])
    qc.cx(flip[4], tile[1])
    qc.cx(flip[4], tile[3])
    qc.cx(flip[4], tile[4])
    qc.cx(flip[4], tile[5])
    qc.cx(flip[4], tile[7])
    qc.cx(flip[5], tile[2])
    qc.cx(flip[5], tile[4])
    qc.cx(flip[5], tile[5])
    qc.cx(flip[5], tile[8])
    qc.cx(flip[6], tile[3])
    qc.cx(flip[6], tile[6])
    qc.cx(flip[6], tile[7])
    qc.cx(flip[7], tile[4])
    qc.cx(flip[7], tile[6])
    qc.cx(flip[7], tile[7])
    qc.cx(flip[7], tile[8])
    qc.cx(flip[8], tile[5])
    qc.cx(flip[8], tile[7])
    qc.cx(flip[8], tile[8])

def week2b_ans_func(lightout4):
    
    address = QuantumRegister(2)
    tile = QuantumRegister(9)
    flip = QuantumRegister(9)
    oracle = QuantumRegister(1)
    result = ClassicalRegister(2)
    auxiliary = QuantumRegister(4)
    qc = QuantumCircuit(address,tile,flip,data,oracle,result,auxiliary)
    
    # initilizing values & address preparation
    qc.h([address[0],address[1]])
    qc.h(flip[:])
    qc.x(oracle[0])
    qc.h(oracle[0])
    qc.barrier()

    ##Qram part

    # address 0 
    if(address[0]==0 and address[1]==0):
        j=0
        for i in lightsout4[0]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()

    # address 1 
    if(address[0]==0 and address[1]==1):
        j=0
        for i in lightsout4[1]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()

    # address 2 
    if(address[0]==1 and address[1]==0):
        j=0
        for i in lightsout4[2]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()
    
    # address 3 
    if(address[0]==1 and address[1]==1):
        j=0
        for i in lightsout4[3]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()
    
    # the number of iterations is 18 for 3x3 grid
    for i in range(18):
        # oracle
        flip_tile(qc,flip,tile)
        # all_zero check
        qc.x(tile[0:9])
        qc.mct(tile[0:9], oracle[0])
        qc.x(tile[0:9])
        
        # U^dagger of flip_tile function is flip_tile itself.
        flip_tile(qc,flip,tile)

        # diffusion
        qc.h(flip)
        qc.x(flip)
        qc.h(flip[8])
        qc.mct(flip[0:8], flip[8])
        qc.h(flip[8])
        qc.x(flip)
        qc.h(flip)
        
    # counter
        
        for i in range (len(flip)):
            qc.mct([flip[i],auxiliary[0],auxiliary[1],auxiliary[2]],auxiliary[3],mode='noancilla')
            qc.mct([flip[i],auxiliary[0],auxiliary[1]],auxiliary[2],mode='noancilla')
            qc.ccx(flip[i],auxiliary[0],auxiliary[1])
            qc.cx(flip[i],auxiliary[0])
            
#         for i in range (len(flip)):
#             qc.cx(flip[i],auxiliary[0])
#             qc.ccx(flip[i],auxiliary[0],auxiliary[1])
#             qc.mct([flip[i],auxiliary[0],auxiliary[1]],auxiliary[2],mode='noancilla')
#             qc.mct([flip[i],auxiliary[0],auxiliary[1],auxiliary[2]],auxiliary[3],mode='noancilla')
# ...only convert aux bits back to 0 state
        qc.barrier()
    
    # the number of iterations is 18 for 3x3 grid
    for i in range(18):
        # oracle
        flip_tile(qc,flip,tile)
        # all_zero check
        qc.x(tile[0:9])
        qc.mct(tile[0:9], oracle[0])
        qc.x(tile[0:9])
        
        # U^dagger of flip_tile function is flip_tile itself.
        flip_tile(qc,flip,tile)

        # diffusion
        qc.h(flip)
        qc.x(flip)
        qc.h(flip[8])
        qc.mct(flip[0:8], flip[8])
        qc.h(flip[8])
        qc.x(flip)
        qc.h(flip)
    
    # Uncompute
    qc.h(oracle[0])
    qc.x(oracle[0])

    ## Reverse Qram part

    # address 3 
    if(address[0]==1 and address[1]==1):
        j=0
        for i in lightsout4[3]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()

    # address 2 
    if(address[0]==1 and address[1]==0):
        j=0
        for i in lightsout4[2]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()

    # address 1 
    if(address[0]==0 and address[1]==1):
        j=0
        for i in lightsout4[1]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()

    # address 0 
    if(address[0]==0 and address[1]==0):
        j=0
        for i in lightsout4[0]:
                qc.cx(i,tile[j])
                j+=1
    qc.barrier()


    #Measure the address
    qc.measure(address[0:2], result[0:2])

    # Reverse the output string.
    qc = qc.reverse_bits()

    return qc
```
