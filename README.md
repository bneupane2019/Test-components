# Test-components 
First time using Git and beginner in python


##This is to check how combined gas power plant with HP performs with CHP
#initialization case
The input for both the technology is taken as 100kWh natural gas.In case of CHP, the plant is considered to have 50\% thermal efficiency and 40\% electrical efficiency. However, incase of GuD it is considered to have 60\% electrical efficiency and one-third of the electrical energy is consumed for a HP with COP of 4.
Both the technologies have reference temperature or ambient temperature of varying from (-10°C to 20) and demand temperature 30°C in first case and in the second case demand temperature varying from (20°C to 80°C).


import numpy as np
import matplotlib.pyplot as plt
# %% Varying ambient temperature
class CCPP:
   
    def __init__(self,Tref,Tdem,E_inputpower,Eefficiency,COP):
      self.Tref = Tref+273
      self.Tdem = Tdem+273
      self.E_inputpower = E_inputpower
      self.Eefficiency= Eefficiency
      self.COP= COP
      self.Electric_output=self.E_inputpower*self.Eefficiency
             
    def thermaloutput(self):
        Thermal_energy =(1/3)*self.Electric_output
        return Thermal_energy
        
    def ambient (self):
        ambient =self.Tref
        return ambient
    
    def qfactor(self):
        Fq= (1-self.Tref/self.Tdem)
        return Fq
  
    def thermalexergyoutput(self):
        Exergy= self.COP*self.qfactor()*self.thermaloutput()
        return Exergy
    
    def exergetic_efficiency(self):
        Eeff= ((self.thermalexergyoutput()+(self.Electric_output-self.thermaloutput()))/self.E_inputpower)*100
        return Eeff
    
    
class CHP:
    
    def __init__(self,Tref,Tdem,E_inputpower,Thermal_efficiency, Electrical_efficiency):
      self.Tref = Tref+273
      self.Tdem = Tdem+273
      self.E_inputpower=E_inputpower
      self.Thermal_energy= self.E_inputpower*Thermal_efficiency
      self.Electrical_output=self.E_inputpower*Electrical_efficiency
       
    def qfactor(self):
        Fq= (1-(self.Tref/self.Tdem))
        return Fq
    
    def ambient (self):
        ambientT =self.Tref
        return ambientT
       
    def thermalexergyoutput(self):
        Exergy= self.Thermal_energy*self.qfactor()
        return Exergy
    
    def exergeticefficiency(self):
        Eeff=((self.thermalexergyoutput()+self.Electrical_output)/self.E_inputpower)*100
        return Eeff
    
ambient_T = np.arange(-10,20,2)   
parameters=CHP(ambient_T,30,100,0.5,0.4)
ChP= (parameters.exergeticefficiency())
values=CCPP(ambient_T,30,100,0.6,4)           
GuD= (values.exergetic_efficiency())

plt.figure (1)
plt.plot(parameters.ambient(),ChP, marker='o', markerfacecolor='blue', markersize=6, color='skyblue', linewidth=2)
plt.plot(values.ambient(),GuD, marker='v',markerfacecolor='red', markersize=4, color='red', linewidth=2)
plt.gca().legend(('CHP','GuD with HP'))
plt.ylabel('Overall Exergetic Efficiency (%)')
plt.xlabel('Ambient Temperature (K)')
plt.title('Overall exergy efficiency with ambient temperature' )
plt.show()

# %%Varying demand temperature
class CCPP:
   
    def __init__(self,Tref,Tdem,E_inputpower,Eefficiency,COP):
      self.Tref = Tref+273
      self.Tdem = Tdem+273
      self.E_inputpower = E_inputpower
      self.Eefficiency= Eefficiency
      self.COP= COP
      self.Electric_output=self.E_inputpower*self.Eefficiency
             
    def thermaloutput(self):
        Thermal_energy =(1/3)*self.Electric_output
        return Thermal_energy
        
    def demand (self):
        demand =self.Tdem
        return demand
    
    def qfactor(self):
        Fq= (1-self.Tref/self.Tdem)
        return Fq
  
    def thermalexergyoutput(self):
        Exergy= self.COP*self.qfactor()*self.thermaloutput()
        return Exergy
    
    def exergetic_efficiency(self):
        Eeff= ((self.thermalexergyoutput()+(self.Electric_output-self.thermaloutput()))/self.E_inputpower)*100
        return Eeff
    
    
class CHP:
    
    def __init__(self,Tref,Tdem,E_inputpower,Thermal_efficiency, Electrical_efficiency):
      self.Tref = Tref+273
      self.Tdem = Tdem+273
      self.E_inputpower=E_inputpower
      self.Thermal_energy= self.E_inputpower*Thermal_efficiency
      self.Electrical_output=self.E_inputpower*Electrical_efficiency
       
    def qfactor(self):
        Fq= (1-(self.Tref/self.Tdem))
        return Fq
    
    def demand (self):
        demand =self.Tdem
        return demand
       
    def thermalexergyoutput(self):
        Exergy= self.Thermal_energy*self.qfactor()
        return Exergy
    
    def exergeticefficiency(self):
        Eeff=((self.thermalexergyoutput()+self.Electrical_output)/self.E_inputpower)*100
        return Eeff
    
demand = np.arange(20,80,5)   
parameters=CHP(10,demand,100,0.5,0.4)
ChP= (parameters.exergeticefficiency())
values=CCPP(10,demand,100,0.6,4)           
GuD= (values.exergetic_efficiency())

plt.figure (1)
plt.plot(parameters.demand(),ChP, marker='o', markerfacecolor='blue', markersize=6, color='skyblue', linewidth=2)
plt.plot(values.demand(),GuD, marker='v',markerfacecolor='red', markersize=4, color='red', linewidth=2)
plt.gca().legend(('CHP','GuD with HP'))
plt.ylabel('Overall Exergetic Efficiency (%)')
plt.xlabel('Demand Temperature (K)')
plt.title('Overall exergy efficiency with demand temperature' )
plt.show()
