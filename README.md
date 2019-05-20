# predicting-the-flower-color
#!/usr/bin/env python
# coding: utf-8



from matplotlib import pyplot as plt
import numpy as np

#each point is length,width,type(0.1)
data=[[3,  1.5, 1],
     [2,   1,   0],
     [4,   1.5, 1],
     [3,   1,   0],
     [3.5, .5,  1],
     [2,   .5,  0],
     [5.5, 1,   1],
     [1,   1,   0]]
mystery_flower=[4.5,1]


# In[5]:


#network
#   o     flowwer type
#  / \    w1,w2,b
# o  o    length,width
#w1=np.random.randn() #random number from the normal function
#w2=np.random.randn()
#b=np.random.randn()



def sigmoid(x):
    return 1/(1+np.exp(-x))
def sigmoid_p(x):
    return sigmoid(x)*(1-sigmoid(x))
  



t=np.linspace(-20,20,50)
y=sigmoid(t)
plt.plot(t,y,c='r')
y_p=sigmoid_p(t)
plt.plot(t,y_p,c='b')


#scatter data
plt.axis([0,6,0,6])# xmin xmax, ymin ymax
plt.grid()
for i in range(len(data)):
    point=data[i]
    color='r'
    if point[2]==0:
        color='b'
    plt.scatter(point[0],point[1],c=color)


learning_rate=0.2
costs=[]
w1=np.random.randn() #random number from the normal function
w2=np.random.randn()
b=np.random.randn()
for j in range(0,50000) :
    ri=np.random.randint(len(data))
    point=data[ri]
    
    #feed forward
    z=w1*point[0]+w2*point[1]+b  #feed forward ;the information moves in only one direction, forward, 
    pred=sigmoid(z)              #from the input nodes, through the hidden nodes
    
    target=point[2]
    cost=np.square(pred-target)
    
    #if j%1000==0:
    #     print(cost)
    #costs.append(cost)
    
    dcost_pred=2*(pred-target)
    dpred_dz=sigmoid_p(z)
    dz_dw1=point[0]
    dz_dw2=point[1]
    dz_db=1
    
    dcost_dz=dcost_pred*dpred_dz
    
    dcost_dw1=dcost_dz*dz_dw1
    dcost_dw2=dcost_dz*dz_dw2
    dcost_db=dcost_dz*dz_db
    
    w1-=learning_rate*dcost_dw1
    w2-=learning_rate*dcost_dw2
    b-=learning_rate*dcost_db
    cost_sum=0
    for l in range(len(data)):
        p=data[l]
        s=p[0]*w1+p[1]*w2+b
        predd=sigmoid(s)
        cost_sum+=np.square(predd-p[2])
    costs.append(cost_sum/len(data))    
plt.plot(costs)    

#seeing the predictions 
for l in range(len(data)):
        p=data[l]
        print(p)
        s=p[0]*w1+p[1]*w2+b
        pred=sigmoid(s)
        print("pred: {}".format(pred))
        

m=mystery_flower[0]*w1+mystery_flower[1]*w2+b
pred=sigmoid(m)
pred


import win32com.client as wincl
speak = wincl.Dispatch("SAPI.SpVoice")



def which_flower(length,width):
    z=length*w1+width*w2+b
    pred=sigmoid(z)
    if pred<.5:
        speak.speak("blue")
    else:
        speak.speak("red")



which_flower(4,1.5)


