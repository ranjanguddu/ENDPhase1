# Project
This project is to write a transformer-based model that can write python code (with proper whitespace indentations). For example:
Input: Write a python function to add two numbers 
Output:
```python
def add_num():
  num1 = 1.5
  num2 = 6.3
  res = num1 + num2
  return res
```
You can find the used sample code file (Data file) what we used to train the model
 [here](https://github.com/ranjanguddu/EVA4-Session-14)

---
## Data Cleaning
---
For data cleaning firstly I tried to split the Data file. But it couldn't work as there are many comments line within the Program. So, finally I did some manual cleaning and create a file name 'Cleaned_Eng_Python_Data.txt' using Regex and then wrote a Python code to break the data in (key,value) : (English, Python) form.
```python
fundict ={}
with open('Cleaned_Eng_Python_Data.txt', 'r') as fR:

    for i in fR.readlines():

        if i.startswith('#') and  ('write' in i.lower() or 'python' in i.lower() or \
            'program' in i.lower() or 'function' in i.lower() or 'generate' in i.lower() or \
            'code' in i.lower() or 'given' in i.lower() or 'find' in i.lower() or 'calculate' in i.lower() or\
            'class' in i.lower() or 'define' in i.lower() or 'check' in i.lower() or 'compute' in i.lower() \
            or 'script' in i.lower() or 'calculate' in i.lower()):
            # print(i)
            key = i[1:]
            

            fundict[key]=''
            continue
        else:
            fundict[key]= fundict[key] + i

print(len(fundict))
```
Let's do some analysis:
```python
src = []
trg = []

max_key_len = 0
max_val_len = 0

src_len = []
trg_len = []

for k,v in fundict.items():
  src_len.append(len(k))
  trg_len.append(len(v))

  src.append(k)
  
  trg.append(v)
  
    
print(f'Key_len:{max(src_len)} and Value: {max(trg_len)}')

```
Let's draw the histogram of Target python Code:
```python
# setting the ranges and no. of intervals 
range = (0, 500) 
bins = 100  
  
# plotting a histogram 
plt.hist(trg_len, bins, range, color = 'green', 
        histtype = 'bar', rwidth = 0.8) 
  
# x-axis label 
plt.xlabel('Python Code Length') 
# frequency label 
plt.ylabel('No. of examples') 
# plot title 
plt.title('Target Histogram') 
  
# function to show the plot 
plt.show() 
```
```
![input](target.png)
```
Let's do with target sequences:
```python
# setting the ranges and no. of intervals 
range = (0, 300) 
bins = 50  
  
# plotting a histogram 
plt.hist(src_len, bins, range, color = 'green', 
        histtype = 'bar', rwidth = 0.8) 
  
# x-axis label 
plt.xlabel('English sentence length') 
# frequency label 
plt.ylabel('No. of examples') 
# plot title 
plt.title('Source Histogram') 
  
# function to show the plot 
plt.show() 
```
```
![input](source.png)
```
We can see from the above histogram that we can cover more than 80 percent sample with source sequence length less than 300 and target sequence length less than 400. Hence we have chosen only those samples having source length less than 300 and target length less than 400. 


```python
src = []
trg = []

for k,v in fundict.items():
  if len(k)<300 and len(v)<400:
    src.append(k)
    trg.append(v)
    
    
print(f'Key_len:{max(src_len)} and Value: {max(trg_len)}, english sentence:{len(src)} and python code:{len(trg)}')
```

