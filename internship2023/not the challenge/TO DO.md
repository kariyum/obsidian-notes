
### api service
- entites
	- [[internship2023/not the challenge/api service/Notes]]
	- slot
	- promotion
	- events
- data model
- CRUD endpoints for every entity

# Notes
1. Prepare statements for all APIs for a faster response time (it skips the parsing phase) [DataStax Java Driver - Prepared statements](https://docs.datastax.com/en/developer/java-driver/3.0/manual/statements/prepared/)
2. Make use of throwing **Exceptions**


# Questions
2. Implement a **GET /all** route for each API ? same for DELETE /all
3. Update routes are not for updating the links, meaning that you cannot link a promotion that was previously linked to slot1 to another slot2. They are for updaing the other columns; name, title...
4. How will we generate data and requests [[Data & Requests generation]]
5. Should we look at cache warmup
6. 