SOURCE CODE 
import cv2 
import torch 
import firebase_admin 
from firebase_admin import credentials, db 
import time 
import threading 
# Global variable to hold the current person count and a lock for thread safety. 
current_count = 0 
count_lock = threading.Lock() 
def firebase_updater(detected_ref): 
global current_count 
while True: 
with count_lock: 
count_val = current_count 
# Update Firebase with the current count 
detected_ref.set(count_val) 
# Wait for 1 second before updating again 
time.sleep(0.5) 
def main(): 
global current_count 
# Initialize Firebase 
cred = credentials.Certificate("key.json") 
firebase_admin.initialize_app(cred, { 
'databaseURL': 'https://firedection-a6dbe-default-rtdb.firebaseio.com/' 
}) 
# Create a reference for the detected humans count in Firebase 
