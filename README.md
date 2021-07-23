1. git status
2. git add merge.sh rebase.sh
3. git commit -m "prepare for merge and rebase"
4. git push origin main
5. git checkout -b git-merge
6. git add merge.sh
7. git commit -m "merge: @ instead *"
8. git add merge.sh
9. git commit -m "merge: use shift"
10. git push origin git-merge
11. git checkout main
12. git add rebase.sh merge.sh
13. git commit -m "changed rebase.sh"
14. git push origin main
15. git log -10
16. git checkout 6866176bd
17. git checkout -b git-rebase
18. git add rebase.sh merge.sh
19. git commit -m "git-rebase 1"
20. git push origin git-rebase
21. git add rebase.sh
22. git commit -m "git-rebase 2"
23. git push origin git-rebase
24. git checkout main
25. git merge git-merge
26. git push origin
27. git checkout git-rebase
28. git rebase -i main
29. git add rebase.sh
30. git rebase --continue
31. git add rebase.sh
32. git rebase --continue
33. git push origin git-rebase
34. git push origin git-rebase -f
35. git checkout main
36. git merge git-rebase
37. git push origin main



