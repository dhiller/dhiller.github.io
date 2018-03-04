---
layout: post
title: Don't forget that thenThrow returns an OngoingStubbing instance *sigh*
---

I've often had the problem that when checking for a working retry mechanism (btw. [spring-retry](https://github.com/spring-projects/spring-retry) works great) I wanted to first throw an exception and then return a value or whatever else is necessary.

Easy peasy, I thought. Just create an Answer and hold state within the answer i.e. a boolean that gets set to true after it has been called:

    @Test
    public void queryIsRetried() {
        final Answer objectAnswer = new Answer() {

            boolean called;

            @Override
            public Object answer(InvocationOnMock invocation) throws Throwable {
                if(called) {
                    return null;
                }

                called = true;
                throw new RuntimeException();
            }
        };
        when(lexdbJdbcTemplate.query(anyString(), any(RowMapper.class)))
                .then(objectAnswer);
        underTest.query("SELECT 1;");
    }

And of course there is a much easier way, stupid me:


    @Test
    public void queryIsRetried() {
        when(lexdbJdbcTemplate.query(anyString(), any(RowMapper.class)))
                .thenThrow(new RuntimeException())
                .thenReturn(null);
        underTest.query("SELECT 1;");
    }
