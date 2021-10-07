---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# Next.js workshop

Client Portal Offsite

---

# How create website in next.js

```shell
- next-project
  - pages
    - index.tsx
```

```shell
/pages/index.tsx => /
/pages/name.tsx => /name
/pages/names/index.tsx => /name
/pages/deep/name.tsx => /deep/name
/pages/[argument].tsx => /<any_name>
/pages/[argument]/[second_argument].tsx => /<any_name>/<any_name>
```

---

# How file looks like

```ts{all|1-3|4-10|10-17|19}
const Page = (props) => {
  return <div>Hello World</div>
}

// Server Side Rendering
export async function getServerSideProps(context: AppContext) {
  return {
    props: {}, // will be passed to the page component as props
  };
}

// Generate static website
export async function getStaticProps(context: AppContext) {
  return {
    props: {}, // will be passed to the page component as props
  }
}

export default SelectDate;
```

---

# Install it

```sh
$ git clone git@github.com:LTe/movie-demo.git
```

or

Use this https://codesandbox.io/s/trusting-andras-wpgb0

---

# Dependencies

```shell
$ yarn install
```

---

# Run application

```shell
$ yarn dev
```

---

# Create first page


```ts
// pages/index.tsx

import { getRange } from '@utils/index';
import { Dates } from '@components/Dates';
import { GetServerSideProps } from 'next';

const SelectDate = () => {
  const range = getRange(5);

  return (
    <div>
      <Dates range={range} />
    </div>
  );
};

export const getServerSideProps: GetServerSideProps = async (context) => {
  return {
    props: {},
  };
};

export default SelectDate;

```

---

# Show cinemas 

```ts
// pages/[date]/index.tsx

import { Cinemas } from '@components/Cinemas';
import { cinemas } from '@utils/cinema-city';
import { GetServerSideProps } from 'next';

const SelectCinema = ({ date }: { date: string }) => {
  return (
    <div>
      <Cinemas cinemas={cinemas} date={date} />
    </div>
  );
};

export const getServerSideProps: GetServerSideProps = async (context) => {
  return {
    props: context.params!,
  };
};

export default SelectCinema;
```

---

# Show Movie (getting data)

```ts
// pages/[date]/[cinema]/index.tsx

import { GetServerSideProps } from 'next';
import { CinemaCityResponse, extendEvents, getMovies } from '@utils/cinema-city';
import { parseISO } from 'date-fns';
import { Movies } from '@components/Movies';

type Props = { data: CinemaCityResponse };
type Params = { date: string; cinema: string };

export const getServerSideProps: GetServerSideProps<Props, Params> = async (
  context
) => {
  const { date, cinema } = context.params!;

  const movies = await getMovies(cinema, parseISO(date));
  return {
    props: {
      data: movies,
    },
  };
};
```

---

# Render movie

```ts
// pages/[date]/[cinema]/index.tsx

// imports

const MoviePage = (props: Props) => {
  const { data } = props;
  const movies = extendEvents(data.body.films, data.body.events);

  return (
    <div>
      <Movies movies={movies} />
    </div>
  );
};

// getServerSideProps

export default MoviePage;
```

---

# Thanks!

Questions?
