
```jsx
import React, { Component, lazy, Suspense } from "react";
import { NavLink, Route, Routes } from "react-router-dom";

import Loading from "./Loading";
const Home = lazy(() => import("./Home"));
const About = lazy(() => import("./About"));

export default class index extends Component {
    render() {
        return (
            <div>
                <div className="switch-bar">
                    <NavLink to="/">Home</NavLink>&nbsp;
                    <NavLink to="/about">About</NavLink>
                </div>
                <Suspense fallback={<Loading />}>
                    <Routes>
                        <Route path="/" element={<Home />} />
                        <Route path="/about" element={<About />} />
                    </Routes>
                </Suspense>
            </div>
        );
    }
}
```