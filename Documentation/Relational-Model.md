# Relational model

## PLACE

place(pk_place, city, state, country, notes)

## ORGANISATION

* Organisation is located_in exactly 1 Place; a Place can have 0..N Organisations.

organisation(pk_organisation, name, type, location, fk_place, notes)

## WRITER

* A Writer is born_in exactly 1 Place; a Place can be birth place for 0..N Writers.

writer(pk_writer, names, dob, gender, wikipedia_link, fk_birth_place, notes)

## WORK

* A Writer writes 0..N Works; each Work is written by 1 Writer.
* A Work is published_in exactly 1 Place; a Place has 0..N Works published in it.

work(pk_work, title, year, genre, fk_writer, fk_published_place, notes)

## AWARD

* Awards exist as entities (name/category/year).
* A Writer can receive 0..N Awards, and an Award can be received by 0..N Writers → M:N.

award(pk_award, name, category, year, notes)

writer_award(pk_writer_award, fk_writer, fk_award, notes)

## MOVEMENT

* A Writer can be member_of 0..N Movements, and a Movement can have 0..N Writers → M:N.

movement(pk_movement, name, period, notes)

writer_movement(pk_writer_movement, fk_writer, fk_movement, notes)

## AFFILIATION (Writer ↔ Organisation)

* A Writer can be affiliated_with 0..N Organisations, and an Organisation can have 0..N Writers → M:N.

writer_organisation(pk_writer_organisation, fk_writer, fk_organisation, notes)
