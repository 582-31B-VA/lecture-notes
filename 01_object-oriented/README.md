# Object-oriented programming

The main idea in object-oriented programming (OOP) is to use objects, or
rather **types** of objects, as the unit of program organization.

Objects combine data values with behavior. They represent information,
but also behave like the things that they represent. The logic of how an
object interacts with other objects is **encapsulated** with the
information that encodes the object's value. When an object is printed,
it knows how to spell itself out in text. If an object is composed of
parts, it knows how to reveal those parts on demand. Objects are both
information and processes, bundled together to represent the properties,
interactions, and behaviors of complex things.

Setting up a program as a number of strictly separated object types
provides a way to think about its structure and thus to enforce some
kind of discipline, preventing everything from becoming entangled.

The way to do this is to think of objects somewhat like you'd think of
an electric mixer or other consumer appliance. The people who design and
assemble a mixer have to do specialized work requiring material science
and understanding of electricity. They cover all that up in a smooth
plastic shell so that the people who only want to mix pancake batter
don't have to worry about all that — they have to understand only the
few knobs that the mixer can be operated with.

Similarly, an **object class** is a subprogram that may contain
arbitrarily complicated code but exposes a limited set of operations
that people working with it are supposed to use. This allows large
programs to be built up out of a number of appliance types, limiting the
degree to which these different parts are entangled by requiring them to
only interact with each other in specific ways. If a problem is found in
one such object class, it can often be repaired or even completely
rewritten without impacting the rest of the program. Even better, it may
be possible to use object classes in multiple different programs,
avoiding the need to recreate their functionality from scratch.
