# WinCo-Food-Inc-Image-Link
Use Python to write codes to count overall image links for WinCo Food Inc and the missing links

import os.path
import os
import pyodbc

#CONSTANTS
LIMITED = False
LIMIT = 600
DEBUG = True

cnxn=pyodbc.connect('DSN=PRGX_GroceryWestAuditMaster_DEV;')
cursor=cnxn.cursor()
cursor.execute("select ImageLink from [PRGX_GroceryWestAuditMaster].[dbo].[Winco_DealImages_PythonBackup]")

if LIMITED: limiter = 0
total = 0
missing = 0
for row in cursor:
    if LIMITED:
        if limiter > LIMIT: break
        else: limiter = limiter + 1
    total += 1
    if total % 500 == 0: print(total)
    # Get cleaned filepath by removing every up until '#'
    clean_file_path = row.ImageLink
    # Get the position index of sign #
    position=clean_file_path.index("#")+1
    file_route=clean_file_path[position:]

    if os.path.isfile(file_route):
        pass
        #print(file_route)
    else:
        missing += 1
        #print("File Missing: " + file_route)

print(f"Missing Files: {missing}")
print(f"Total Files: {total}")
print(f"Coverage: {missing/total}")
