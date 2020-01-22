# DisciplineTest



    package com.example.server;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
    import org.springframework.dao.DataIntegrityViolationException;

    import javax.validation.ConstraintViolation;
    import javax.validation.Validation;
    import javax.validation.Validator;
    import javax.validation.ValidatorFactory;

    import com.example.server.Discipline.entity.Discipline;
    import com.example.server.Discipline.repository.DisciplineRepository;

    import java.util.Optional;
    import java.util.Set;

    import static org.junit.jupiter.api.Assertions.assertEquals;
       import static org.junit.jupiter.api.Assertions.assertThrows;

    @DataJpaTest
    public class DisciplineTest {
    private Validator validator;

    @Autowired
    DisciplineRepository DisciplineRepository;

    @BeforeEach
    public void setup() {
        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        validator = factory.getValidator();
    }

    @Test // กรณี 1 Discipline OK
    void B5801206_testCreateDisciplineOK() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(50L);
        discipline.setSince("term2 2562");
        discipline.setUntil("term3 2562");

        discipline = DisciplineRepository.saveAndFlush(discipline);

        Optional<Discipline> DisciplineCreated = DisciplineRepository
                .findById(discipline.getDisciplineId());

        assertEquals(1L, DisciplineCreated.get().getDisciplineId());
        assertEquals(2562L, DisciplineCreated.get().getSchoolyear());
        assertEquals(50L, DisciplineCreated.get().getPoint());
        assertEquals("term2 2562", DisciplineCreated.get().getSince());
        assertEquals("term3 2562", DisciplineCreated.get().getUntil());
    }

    @Test // กรณี 2 DisciplineId must not be null
    void B5801206_testDisciplineIdMustNotBeNull() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(null);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(50L);
        discipline.setSince("term2 2562");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must not be null", v.getMessage());
        assertEquals("disciplineId", v.getPropertyPath().toString());
    }

    @Test // กรณี 3 Schoolyear must not be null
    void B5801206_testSchoolyearMustNotBeNull() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(null);
        discipline.setPoint(50L);
        discipline.setSince("term2 2562");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must not be null", v.getMessage());
        assertEquals("schoolyear", v.getPropertyPath().toString());
    }

    @Test // กรณี 4 Point must not be null
    void B5801206_testPointMustNotBeNull() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(null);
        discipline.setSince("term2 2562");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must not be null", v.getMessage());
        assertEquals("point", v.getPropertyPath().toString());
    }

    @Test // กรณี 5 Since must not be null
    void B5801206_testSinceMustNotBeNull() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(50L);
        discipline.setSince(null);
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must not be null", v.getMessage());
        assertEquals("since", v.getPropertyPath().toString());
    }

    @Test // กรณี 6 Until must not be null
    void B5801206_testUntilMustNotBeNull() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(50L);
        discipline.setSince("term2 2562");
        discipline.setUntil(null);

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must not be null", v.getMessage());
        assertEquals("until", v.getPropertyPath().toString());
    }

    @Test // กรณี 7 Schoolyear must not be 5 digit
    void B5801206_testSchoolyearNotBe5Digit() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(12345L);
        discipline.setPoint(50L);
        discipline.setSince("term2 2562");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must match \"\\d{4}\"", v.getMessage());
        assertEquals("schoolyear", v.getPropertyPath().toString());
    }

    @Test // กรณี 8 Schoolyear must not be 3 digit
    void B5801206_testSchoolyearNotBe12Digit() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(123L);
        discipline.setPoint(50L);
        discipline.setSince("term2 2562");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("must match \"\\d{4}\"", v.getMessage());
        assertEquals("schoolyear", v.getPropertyPath().toString());
    }

    @Test // กรณี 9 Since must not less than 4
    void B5801206_testSinceMustNotLessThan4() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(50L);
        discipline.setSince("123");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("size must be between 4 and 10", v.getMessage());
        assertEquals("since", v.getPropertyPath().toString());
    }

    @Test // กรณี 10 Since must not more than 10
    void B5801206_testSinceMustNotMoreThan10() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(123L);
        discipline.setPoint(50L);
        discipline.setSince("12345678901");
        discipline.setUntil("term3 2562");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("size must be between 4 and 10", v.getMessage());
        assertEquals("since", v.getPropertyPath().toString());
    }

    @Test // กรณี 11 Until must not less than 4
    void B5801206_testUntilMustNotLessThan4() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(2562L);
        discipline.setPoint(50L);
        discipline.setSince("term3 2562");
        discipline.setUntil("123");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("size must be between 4 and 10", v.getMessage());
        assertEquals("until", v.getPropertyPath().toString());
    }

    @Test // กรณี 12 Until must not more than 10
    void B5801206_testUntilMustNotMoreThan10() {
        Discipline discipline = new Discipline();
        discipline.setDisciplineId(1L);
        discipline.setSchoolyear(123L);
        discipline.setPoint(50L);
        discipline.setSince("term3 2562");
        discipline.setUntil("12345678901");

        Set<ConstraintViolation<Discipline>> result = validator.validate(discipline);

        assertEquals(1, result.size());

        ConstraintViolation<Discipline> v = result.iterator().next();
        assertEquals("size must be between 4 and 10", v.getMessage());
        assertEquals("until", v.getPropertyPath().toString());
    }

}
