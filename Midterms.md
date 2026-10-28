- [x] CSC311:  Thur, Oct 8, 7:00 - 10:00 pm EX200
- [x] APM236: Wed, Oct 14, 12:00 - 12:45 pm MS3154
- [x] MAT235: Mon, Oct 19, 7:10 - 8:40 pm
- [x] CSC236:  Thur, Oct 22, 8:00 - 9:30 pm

- [ ] CSC311:  Fri, Nov 20, 4:00 - 6:00 pm
- [ ] APM236: Wed, Nov 25, 6:15 - 7:45 pm
- [ ] MAT235: Fri, Nov 27, 6:10 - 7:40 pm


- initialize with `{}`
- `nullptr` keyword instead of null
- `static_cast<type *>(raw ptr)`
- `new/delete` instead of malloc (calls malloc eventually but more safety checks)
	- `new(std::nothrow)` to simply return `nullptr` on fail
- `include <memory>` header
	- `std::unique_ptr<int> uPtr = std::make_unique<int>(7)`
		- `std::unique_ptr<int> movedPtr = std::move(uPtr)` only way to move the pointer to another variable, simple assignment would not compile
			- Original variable is set to `nullptr`
		- pointer is automatically freed once out of scope
		- When returning these from functions, will automatically use move semantics to move it to calling function 
	- `std::shared_ptr<int> sPtr1 = std::make_shared<int>(500)`
		- `std::shared_ptr<int> sPtr2 = sPtr1` copying is allowed
		- `sPtr1.use_count()` number of vars holding the pointer
		- pointer is automatically freed once **all** variables holding pointer are out of scope
	- dereferencing syntax is the same as C

- `std::array` from `<array>`
- `std::views`
- `std::ranges`
- `std::string` from `<string>`
- references, ie. `int& p`, syntactic sugar for pointer const (doesn't even store the pointer it copies, is purely syntax)
