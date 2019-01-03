---
layout: post
title:	"Functors and Film Strips"
date:	2018-08-20
category: math
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

$$\begin{align}
\Phi(f) \circ \Phi(g) = \Phi(f \circ g)
\end{align}$$ <!--_ -->
{: style="text-align: center"}

<!--exc-->

## Summary

A video can be represented in two different ways:
as a film strip, which lays out a sequence of frames across space,
or as a movie, which lays out a sequence of frames across time.
Each representation has its uses:
editing a video as it's playing is basically impossible;
enjoying a video as a film strip is challenging.
When needed, videos are (or used to be) edited
by first turning them into strips of film,
then editing the film,
and then projecting the film as a movie.
By implementing this process in Python
and then describing it mathematically,
we'll reinvent the concept of _functors_.

### Motivation

While working on a machine learning project recently,
I ran into a common problem:
the code base I wanted to work with made a few assumptions
that didn't fit my situation, and so it refused to run.
Specifically, the code was written to work with _image_ data,
but I was working with _movie_ data.
That is, the code expected arrays with three dimensions,
height, width, and channel, but I was working with
arrays wih four dimensions, frame, height, width, and channel.

I resolved this problem by using the same trick as a projectionist:
I converted the movies into wide images, like strips of film,
applied the code to them,
and then converted the film strips back into images.

This is a trick general enough to not only have a name,
but also to have a ``pure math'' form:
it is known as _lifting_ a function with a _functor_.

### Film Editing 101

So imagine we'd like to alter or edit video data that
is available only as a movie.

First, let's consider the movie-to-film process.
If we have a movie and we want to convert it to film,
we must record it, e.g. with a camera.
Let's abstract this process as `capture`,
as in `film = capture(movie)`.

At this point, we can apply any transformation
that operates on films.
For example, we can correct the colors, frame by frame,
or we can add subtitles,
which is much easier in less than real time.
The result is an edited film, which we write
`edited_film = f(film)`,
where `f` is a function that takes in film strips
and returns film strips.

Finally, we must convert the (edited) film strip back into a movie
if we want to enjoy the fruits of our labor.
This is done by `project`ing the film strip by passing it
in front of a synchronized light source that converts each frame
in the strip into a projected frame in sequence.
If we wanted to write this process the
same way we did the previous two steps,
we might write
`movie = project(film)`,
where `film` can be any film strip whose aspect ratio
matches the settings on our projection apparatus.

### Concrete Implementation in Python

Once we've agreed on a way to represent movies and film strips,
we can go about writing a computer-friendly version of the editing process,
which is both a nice springboard to thinking about it mathematically
and a useful exercise to check our work.
Computers are like diligent but not-too-bright students
who listen to everything we say and nothing more,
and bring nothing of their own to the table.
If we cannot get them to understand it, we're missing something
in our description.

```python
def capture(movie):
    film = np.hstack(movie)
    return film
```

```python
def project(film):
    ASPECT_RATIO = 1  # projector settings
    frame_height, film_width, _ = film.shape
    num_frames = film_width / height / ASPECT_RATIO
    movie = np.split(film, num_frames)
    return movie
```

```python
def lift_to_movies(film_function):
    def movie_function(movie):
    	edited_movie = project(film_function(capture(movie)))
    	return edited_movie
    return movie_function
```


### Spaces of Vectors Have Nice Properties

["How the Simplex is a Vector Space"](https://golem.ph.utexas.edu/category/2016/06/how_the_simplex_is_a_vector_sp.html),
[using pushforwards to succinctly state the business of statistics]({{site.url}}/stats/2017/02/24/statistics-as-pushforward.html).

[matrix_norms_4]: {{site.imgurl}}/matrix_norms_4.png
